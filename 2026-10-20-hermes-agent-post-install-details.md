---
title: Hermes Agent Post Install Details
date: 2026-10-20 00:50 +0100
---

Your Docker mount routing is working as intended: the file was created by the agent inside its container and written directly to your 1 TB `/data` volume without consuming space on your root disk (`/dev/sda1`).

Here is how to connect **Zoho Mail** and set up an automated **Morning Executive Briefing** to complete your assistant setup.

---

### Part 1: Connect Zoho Mail (IMAP / SMTP)

Hermes handles mail via Python's standard `smtplib` and `imaplib`. Configure the credentials in `~/.hermes/.env`.

1. **Open the environment file:**
```bash
nano ~/.hermes/.env

```


2. **Add your Zoho Mail parameters to the bottom:**
```env
EMAIL_ADDRESS=you@yourdomain.com
EMAIL_PASSWORD=your_16_char_zoho_app_password
EMAIL_SMTP_HOST=smtppro.zoho.eu
EMAIL_SMTP_PORT=465
EMAIL_IMAP_HOST=imappro.zoho.eu
EMAIL_IMAP_PORT=993
EMAIL_USE_SSL=true

```


*(If your account is registered under the global US data center instead of Europe, use `smtppro.zoho.com` and `imappro.zoho.com`).*
3. Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).
4. **Restart the gateway daemon:**
```bash
hermes gateway restart

```


*Verification:* In Telegram, ask the bot:
`"Check my Zoho inbox and tell me if I have any unread emails from today."`
The bot will invoke its email tool and return your message count or subject lines without raising authentication errors.

---

### Part 2: Schedule an Automated Daily Morning Briefing

Hermes includes a native cron scheduler that routes directly to your Telegram chat (`home channel`).

You can schedule it conversationally directly from Telegram:

1. **Send this message to Armature Ops in Telegram:**
```text
Schedule a daily cron job for 07:30 every weekday. 
When it runs:
1. Search the web for top UK tech, business, and AI headlines.
2. Check my inbox for priority unread emails.
3. Synthesize this into a structured 5-point executive morning briefing and send it to me here.

```


2. **Or verify it directly from your VPS CLI:**
```bash
hermes cron list

```


*Verification:* Output displays the newly scheduled cron entry, its cron expression (`30 7 * * 1-5`), and target destination.

---

### Part 3: Operational Quick Reference

Keep these commands handy for maintaining your setup:

* **Monitor live execution logs:**
```bash
journalctl --user -u hermes-gateway -f

```


* **Restart the assistant service:**
```bash
hermes gateway restart

```


* **Verify disk utilization on your Storage Box:**
```bash
df -h /data

```


◆ Tool Availability Summary
  6/10 tool categories available:

   ✓ Vision (image analysis)
   ✗ Web Search & Extract (missing EXA_API_KEY, PARALLEL_API_KEY, FIRECRAWL_API_KEY/FIRECRAWL_API_URL, TAVILY_API_KEY, PERPLEXITY_API_KEY, KEENABLE_API_KEY, or SEARXNG_URL)
   ✓ Browser Automation (Local browser)
   ✗ Image Generation (missing FAL_KEY or OPENAI_API_KEY)
   ✓ Text-to-Speech (Google Gemini)
   ✗ Speech-to-Text (Local Whisper — not installed) (missing run 'hermes tools' → Speech-to-Text)
   ✗ Skills Hub (GitHub) (missing GITHUB_TOKEN)
   ✓ Terminal/Commands
   ✓ Task Planning (todo)
   ✓ Skills (view, create, edit)

⚠ Some tools are disabled. Run 'hermes setup tools' to configure them,
⚠ or edit ~/.hermes/.env directly to add the missing API keys.


┌─────────────────────────────────────────────────────────┐
│              ✓ Setup Complete!                          │
└─────────────────────────────────────────────────────────┘

📁 All your files are in ~/.hermes/:

   Settings:  /home/tope/.hermes/config.yaml
   API Keys:  /home/tope/.hermes/.env
   Data:      /home/tope/.hermes/cron/, sessions/, logs/

────────────────────────────────────────────────────────────

📝 To edit your configuration:

   hermes setup          Re-run the full wizard
   hermes setup model    Change model/provider
   hermes setup terminal Change terminal backend
   hermes setup gateway  Configure messaging
   hermes setup tools    Configure tool providers

   hermes config         View current settings
   hermes config edit    Open config in your editor
   hermes config set <key> <value>
                          Set a specific value

   Or edit the files directly:
   nano /home/tope/.hermes/config.yaml
   nano /home/tope/.hermes/.env

────────────────────────────────────────────────────────────

🚀 Ready to go!

   hermes              Start chatting
   hermes gateway      Start messaging gateway
   hermes doctor       Check for issues

✓ Shared metrics off. Change it any time with `hermes setup telemetry`.
✓ Gateway service already installed
✓ Hermes Agent install complete. Run: hermes
→ Reload your shell to use hermes: open a new terminal, or run: source ~/.bashrc
