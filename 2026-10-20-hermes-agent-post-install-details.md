---
title: Hermes Agent Post Install Details
date: 2026-10-20 00:50 +0100
tags: []
---

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
