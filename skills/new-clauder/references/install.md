# Install / pre-flight reference

Only use this if the user says they have not installed Claude Code yet, or seems unsure whether it's running on their machine.

If they are already chatting with you in Claude Code, they are already inside it — say so explicitly: **"You're talking to me through Claude Code right now. It's already installed and running. We're good to go."** Then return to Step 1 of the tour.

---

## Pre-install checks (especially for InfoSec audiences)

Before they touch the installer:

- **Work laptop with corporate management?** Stop and ask IT/security. Most companies now have a vendor-approval process for AI tools, and MDM/DLP/EDR agents may block the install or log every file Claude reads.
- **Sensitive data on the machine** — client files, PHI, regulated material, NDA-protected docs? Same answer.
- **Personal machine + non-sensitive learning?** Continue.
- **Personal machine + work data in the folder?** Treat it like a work laptop.

---

## Heads up: Claude Code requires a paid Claude plan

The free Claude plan at claude.ai does **not** include Claude Code. To use it, the user needs one of:

- **[Claude Pro](https://www.anthropic.com/pricing)** — about $20/month. The entry point. Plenty for learning, light/moderate daily use, most personal projects. Default recommendation.
- **[Claude Max](https://www.anthropic.com/pricing)** — tiered, starts around $100/month. For daily-driver power users with substantially higher limits. Not where beginners should start.
- **API credits** — pay-as-you-go via [console.anthropic.com](https://console.anthropic.com/). Useful for people who'd rather meter by tokens than commit monthly.

Surface this *before* they create an Anthropic account, not after — saves the "wait, I have to pay?" moment.

If they're hesitant, suggest they try the free chat at [claude.ai](https://claude.ai) for a few days first to see if they like how Claude thinks. The free plan won't run Claude Code (the agent on their machine), but it's the same brain — paid plan unlocks the hands.

---

## If they genuinely haven't installed yet

Ask which OS: **macOS, Windows, or Linux.**

### macOS

Two ways:

1. **The desktop app** — download from `claude.com/claude-code`. **Recommended for non-developers.** Open it, sign in, done. No Node, no npm, no Homebrew.
2. **The CLI** — if they want the terminal version:
   ```
   npm install -g @anthropic-ai/claude-code
   ```
   Requires Node.js. Easiest install via Homebrew:
   ```
   brew install node
   ```
   Then run `claude` in any terminal.

If they don't have Homebrew, point them to `brew.sh` and offer to walk through it — but suggest the desktop app instead if they're not committed to the CLI path.

### Windows

Two ways:

1. **The desktop app** — same URL: `claude.com/claude-code`. Recommended path.
2. **The CLI** — needs Node.js. Install via [winget](https://learn.microsoft.com/en-us/windows/package-manager/winget/) (`winget install OpenJS.NodeJS`) or download from [nodejs.org](https://nodejs.org/). Then run `claude` in PowerShell or Windows Terminal.

WSL (Windows Subsystem for Linux) is an option for terminal die-hards but flag it as a "set aside an hour" project for newcomers.

### Linux

`npm install -g @anthropic-ai/claude-code` after Node is installed via their distro's package manager (apt, dnf, pacman).

---

## Confirming it worked

After install, they should be able to:

1. Open the app (or run `claude` in a terminal).
2. Sign in with their Anthropic account.
3. See a chat prompt waiting for input.
4. Type `/help` and see a menu pop up (this is the decisive test — if `/help` shows a menu, they're in Claude Code).

If `/help` does nothing or just prints `/help` as text, they're probably in claude.ai (the browser chat) — not Claude Code. Point them to the desktop app.

If the app prompts for admin rights during install and they're on a managed laptop, stop. Do not enter someone else's credentials. Send them back to the pre-install check.

---

## What "you're already inside it" looks like

If you (Claude) are reading this skill, the user is talking to you through Claude Code. They are inside the tool. They might not realize this. A single sentence — "you're already in Claude Code; this chat IS the tool" — saves them ten minutes of confusion.
