---
title: Hermes Agent Process Specification
date: 2026-10-20 08:53 +0100
tags: []
---

```markdown
# PROCESS_SPEC.md

## System Specification & Operational Decision Log: Hermes Agent Deployment & Multi-Identity Mail Pipeline
- **Host Target:** Linux VPS (`aolite`) | 4 vCPU | 8 GB RAM | 80 GB Local SSD (`/dev/sda1`)
- **Remote Target:** 1 TB CIFS Mount (`/data` via Hetzner Storage Box `//u651995.your-storagebox.de/backup`)
- **Runtime Environment:** Docker Container Sandbox with Tailscale network perimeter
- **Core Orchestrator:** Hermes Agent (`v0.22.2`) via `systemd --user`
- **Primary Control Surface:** Telegram Bot (`Armature Ops`)
- **Outbound Communication Infrastructure:** Zoho Europe SMTP (`smtppro.zoho.eu`) with dynamic alias routing

---

## 1. Stage: Filesystem Architecture & Symlink Isolation

### Sequence of Actions & Trigger Conditions
1. Reject wholesale symlinking of application state directories (`~/.hermes -> /data/hermes_data`).
2. Retain local Python runtime, virtual environments (`uv`), and state SQLite DBs on local NVMe/SSD root (`/home/tope/.hermes`).
3. Explicitly isolate high-volume artifacts (documents, reports, scraped outputs, synthesized media) to remote network storage (`/data/hermes_output/`).

### Hand-offs & Boundaries
- Host OS (`systemd`) manages the local runtime located at `/home/tope/.local/share/uv/`.
- Container mounts map `/data/hermes_output/documents` to internal container workspaces (`/output`, `/workspace`).

### Operational Rationale & Trade-offs
- **Accepted Trade-off:** Local root disk utilization (~2–3 GB for runtime dependencies and PyPI caches) accepted to eliminate CIFS/SMB remote lock latency and fatal cross-device symlink failures (`os error 5: Input/output error`).
- **Storage Box Role:** Designated strictly for append/read-heavy bulk payload deliverables where POSIX-compliant symlinking is not required by runtime engines.

### Execution Guardrails & Error Handling
- **Failure Mode:** `uv venv failed: Input/output error (os error 5)` during package compilation.
- **Remediation Pathway:**
  1. Unlink: `rm ~/.hermes`
  2. Purge remote target cache: `rm -rf /data/hermes_data`
  3. Reinstall on local filesystem: `/home/tope/.hermes`
- **Reversibility:** Local runtime directory can be completely purged with `rm -rf ~/.hermes` without risking the integrity of `/data` mounts.

### Open Flags & Dependencies
- Local root storage limit: 8.2 GB remaining on `/dev/sda1`. Periodic cache invalidation required:
  ```bash
  uv cache clean && sudo apt autoremove -y && sudo apt clean

```

---

## 2. Stage: Installation Stream & Non-Interactive Input Capture

### Sequence of Actions & Trigger Conditions

1. Download installer script: `curl -fsSL https://hermes-agent.nousresearch.com/install.sh`.
2. Prohibit unbuffered piping (`... | bash`) when interactive configuration is expected.
3. Decouple base dependency installation from the interactive CLI wizard (`hermes setup`).

### Operational Rationale & Trade-offs

* **Accepted Trade-off:** Two-step execution process (automated binary fetch followed by explicit `hermes setup` invocation) accepted over single-line "one-click" installers.
* **Why:** Piped stdin consumption causes premature wizard cancellation (`Setup cancelled. Remaining sections were not changed`) because standard input is exhausted by the curl stream.

### Execution Guardrails & Error Handling

* **Failure Mode:** Setup silently aborts after installing dependencies; fails to prompt for provider, model, or backend choices.
* **Remediation Pathway:**
1. Terminate or cancel broken background prompt (`Ctrl+C` or `n`).
2. Refresh shell environment: `source ~/.bashrc` (ensure `~/.local/bin` is in `$PATH`).
3. Re-enter setup explicitly via TTY: `hermes setup`.



### Open Flags & Dependencies

* Runtime prerequisite: Node.js requires `libatomic.so.1` on headless debian/ubuntu targets (`sudo apt install -y libatomic1`).

---

## 3. Stage: Sandbox Isolation & Network Perimeter Policy

### Sequence of Actions & Trigger Conditions

1. Select `Docker / Podman` backend for shell tool execution.
2. Select `No` on sandbox egress firewall installation (`iron-proxy`).
3. Explicitly expose required runtime environment variables to the sandbox via `docker_forward_env`.

### Hand-offs & Boundaries

* Terminal tool commands issued by LLM execute strictly within an isolated container runtime.
* Gateway, cron orchestration, and messaging webhooks run on the host user-space process.

### Operational Rationale & Trade-offs

* **Accepted Trade-off:** Unrestricted outbound container network egress accepted over localized proxy filtering.
* **Why:** The assistant requires unrestricted HTTPS access for dynamic web scraping, research endpoints, and remote SMTP connections. Egress firewalling introduces proxy token substitution that breaks internal Tailscale subnet routing (`100.x.y.z`) and dynamic API handshakes.
* **Security Boundary:** Perimeter protection is enforced upstream via Tailscale host binding rather than localized container egress packet inspection.

### Execution Guardrails & Error Handling

* **Failure Mode:** Missing host environment variables inside the execution container; `EMAIL_PASSWORD` and API keys undefined during script execution.
* **Remediation Pathway:** Do not rely on top-level `env:` tags in `config.yaml`. Use `docker_forward_env` within the `terminal:` block:
```yaml
terminal:
  docker_forward_env:
    - "EMAIL_ADDRESS"
    - "EMAIL_PASSWORD"
    - "EMAIL_SMTP_HOST"
    - "EMAIL_SMTP_PORT"

```



### Open Flags & Dependencies

* User account `tope` must be an active member of the local `docker` group to execute commands without sudo password intervention:
```bash
sudo usermod -aG docker $USER

```



---

## 4. Stage: Messaging Surface & Telegram Orchestration

### Sequence of Actions & Trigger Conditions

1. Stand up Telegram bot via `@BotFather` (`Armature Ops`).
2. Reject automated third-party onboarding proxies (`@NousHostedHermesBot`) to preserve bot administrative ownership and avoid orphan handle locks.
3. Restrict incoming message evaluation strictly to explicit user IDs (`TELEGRAM_ALLOWED_USERS`).
4. Designate authenticated numeric ID as the primary system `home_channel`.

### Hand-offs & Boundaries

* Telegram API -> Long-polling outbound worker thread on host -> Internal agent task queue -> Docker container sandbox execution.

### Operational Rationale & Trade-offs

* **Accepted Trade-off:** Manual bot token creation via `@BotFather` accepted over QR automated setup to guarantee persistent DNS/handle recovery and administrative rights.
* **Why:** Automatic provisioning bots lock the username behind an external account; if the initial token exchange times out, the username cannot be reclaimed.

### Execution Guardrails & Error Handling

* **Failure Mode:** `Gateway hit a non-retryable startup conflict: telegram: Telegram bot token rejected`.
* **Remediation Pathway:**
1. Re-generate bot token directly with `@BotFather` via `/token`.
2. Write unquoted string to `~/.hermes/.env`: `TELEGRAM_BOT_TOKEN=<token>`.
3. Validate allowed user lock: `TELEGRAM_ALLOWED_USERS=<numeric_id>`.
4. Reload daemon: `hermes gateway restart`.


* **Failure Mode:** Premature SSH logout terminates user-level systemd processes.
* **Remediation Pathway:**
```bash
sudo loginctl enable-linger $USER

```



### Open Flags & Dependencies

* Ensure `drop_pending_on_cold_boot: true` is retained in Telegram platform defaults to avoid replaying duplicate execution commands upon service restart.

---

## 5. Stage: Toolset Optimization & Search/Scrape Pipeline

### Sequence of Actions & Trigger Conditions

1. Strip non-essential toolsets (`computer_use`, `discord`, `yuanbao`, `image_gen`) from active configuration.
2. Route web search operations to Tavily API (`search_backend: "tavily"`).
3. Route web page parsing and deep markdown extraction to Tavily (`extract_backend: "tavily"`).
4. Direct fallback browser operations to Local Headless Chromium (`Local Browser`).

### Operational Rationale & Trade-offs

* **Accepted Trade-off:** API rate limits (Tavily free tier 1,000 requests/mo) accepted over zero-cost DuckDuckGo (`ddgs`).
* **Why:** DuckDuckGo lacks page content extraction (`ddgs is a search-only backend`) and regularly triggers upstream HTTP parsing failures (`DecodeError` on fatal alerts). Tavily delivers structured markdown payloads ready for LLM context injection without scraping overhead.
* **Headless Cleanup:** Disabling `computer_use` eliminates missing dependency crashes for `cua-driver` (which requires X11 libraries like `libX11.so.6` absent on headless servers).

### Execution Guardrails & Error Handling

* **Failure Mode:** Dependency staging error on package manager sync: `✗ cua-driver: error while loading shared libraries: libX11.so.6`.
* **Remediation Pathway:** Explicitly bypass GUI driver installation:
```bash
hermes pm install --without cua-driver

```



### Open Flags & Dependencies

* Require `TAVILY_API_KEY` present in `~/.hermes/.env`.

---

## 6. Stage: Outbound Media & Storage Mapping

### Sequence of Actions & Trigger Conditions

1. Map host Storage Box subdirectories directly into Docker runtime volume definitions.
2. Map both `/workspace` and `/output` inside the sandbox to `/data/hermes_output/documents`.
3. Mirror the host TTS cache `/home/tope/.hermes/cache` inside the sandbox runtime.

### Schemas & Mount Configuration (`~/.hermes/config.yaml`)

```yaml
terminal:
  backend: "docker"
  cwd: "."
  timeout: 180
  home_mode: "auto"
  docker_mount_cwd_to_workspace: false
  docker_volumes:
    - "/data/hermes_output/documents:/output"
    - "/data/hermes_output/documents:/workspace"
    - "/data/hermes_output/audio:/home/tope/.hermes/cache/audio"
    - "/home/tope/.hermes/cache:/home/tope/.hermes/cache"

```

### Operational Rationale & Trade-offs

* **Accepted Trade-off:** Container-generated files written directly to network-attached storage mount instead of isolated ephemeral container layers.
* **Why:** Eliminates the `Docker MEDIA path ... did not resolve to a host sandbox file` failure where the host gateway could not read generated audio notes or text files to transmit them over Telegram.

### Execution Guardrails & Error Handling

* **Failure Mode:** Ruamel YAML parsing failure due to duplicate root blocks: `found duplicate key "terminal" with value "{}"`.
* **Remediation Pathway:**
1. Inspect file: `nano ~/.hermes/config.yaml`.
2. Remove redundant manual terminal definitions created during onboarding.
3. Verify schema validity: `hermes config check`.



---

## 7. Stage: Multi-Identity Mail Pipeline & SMTP Dispatch

### Sequence of Actions & Trigger Conditions

1. Authenticate master connection using root mailbox credentials (`tope@armatureops.co.uk`) via Zoho Europe.
2. Direct all outbound SMTP operations strictly to **Port 587 (STARTTLS)**.
3. Reject direct SSL **Port 465** for host networking (blocked/filtered upstream by VPS provider firewall).
4. Direct incoming IMAP monitoring strictly to **Port 993 (SSL)** via `imappro.zoho.eu`.
5. Permit dynamic alias impersonation on outbound headers (`From:`) matching the operational persona.

### Sender Identity Matrix

* Master Authenticated Account: `tope@armatureops.co.uk`
* Supported Verified Aliases:
* `partnerships@media.armatureops.co.uk` (Outreach / Brand Briefs)
* `director@armatureops.co.uk` (Executive Governance)
* `hello@armatureops.co.uk` (General Inquiries)
* `stories@thenigerianodyssey.com` (Editorial Dispatches)
* `hi@topearebi.com` (Personal Professional)



### Environmental Variables (`~/.hermes/.env`)

```env
EMAIL_ADDRESS=tope@armatureops.co.uk
EMAIL_PASSWORD=<zoho_16_char_app_specific_password>
EMAIL_IMAP_HOST=imappro.zoho.eu
EMAIL_IMAP_PORT=993
EMAIL_SMTP_HOST=smtppro.zoho.eu
EMAIL_SMTP_PORT=587
EMAIL_USE_SSL=false
EMAIL_USE_STARTTLS=true
EMAIL_TRUST_FROM_HEADER=true

```

### Operational Rationale & Trade-offs

* **Accepted Trade-off:** App-specific passwords mandatory over standard user credentials.
* **Why:** Multi-Factor Authentication (MFA) on Zoho accounts outright terminates generic password attempts with `[AUTHENTICATIONFAILED] Invalid credentials(Failure)`.
* **Port Strategy:** Port 587 with STARTTLS avoids the silent TCP drop/connection timeout (`[Errno 110] Connection timed out`) occurring on Port 465.

### Execution Guardrails & Error Handling

* **Failure Mode 1: Authentication Failure on IMAP/SMTP.**
* *Remediation:* Confirm IMAP access is toggled **ON** in Zoho Mail Web settings (`Mail Accounts -> IMAP Access`). Verify password is an App Password generated at `accounts.zoho.eu`.


* **Failure Mode 2: Client Communications Wrapped in Cron Metadata.**
* *Symptom:* Email contains `Cronjob Response: ... (job_id: ...) ... To stop or manage this job`.
* *Root Cause:* Directing message delivery through the Hermes Reminder/Cron scheduler target (`email:recipient@domain.com`) wraps output in administrative notification templates.
* *Remediation & System Rule:* External client communications must be generated as clean MIME payloads via raw Python `smtplib` with STARTTLS, never routed through the platform reminder notification wrapper.
* *Pruning Command:* `hermes cron delete <job_id>`.



---

## 8. State Verification & Diagnostics Runbook

Execute this verification sequence after any configuration adjustment or service restart:

```bash
# 1. Verify Configuration Integrity
hermes config check

# 2. Re-align Dependencies (Bypassing GUI CUA Drivers)
hermes pm install --without cua-driver

# 3. Restart Host Service Daemon
hermes gateway restart

# 4. Confirm Service Active Status
hermes status

# 5. Tail Production Logs for Clean Polling Handshakes
journalctl --user -u hermes-gateway -n 20 --no-pager

```

**Definition of Done (DoD) Indicators:**

* `hermes status` displays `Gateway: ✓ running`.
* Journal logs display:
* `[Telegram] Connected to Telegram (polling mode)`
* `[Email] Connected as tope@armatureops.co.uk`
* Zero `duplicate key`, `did not resolve to a host sandbox file`, or `IMAP connection failed` entries.


* Test artifacts materialize on `/data/hermes_output/documents/` upon execution within Telegram.

```

```
