# "Where am I running this?" — surface-by-surface reference

Most newcomer confusion isn't about *what Claude does*, it's about *which Claude they're in*. Use this reference any time the user gets tangled up between Claude Code (this chat, where NewClauder lives), regular Claude (the website), and the various ways to run Claude Code itself.

Read the section that matches what the user reports seeing.

---

## The decisive test

If the user can type `/help` and a menu pops up — they're in **Claude Code**. Everything in this tour applies.

If `/help` does nothing (or just prints `/help` as text), they're in **regular Claude** at claude.ai. Different product. Slash commands don't work there, file edits don't work there, this tour doesn't fully apply there.

That one keystroke answers 90% of "where am I?" questions.

---

## The surfaces, plain English

### Claude Code — desktop app

A Mac or Windows app. Looks like a regular app on your computer. Has a folder picker so you tell it which project to work on. Best starting point for non-technical users.

- Slash commands: ✅
- Local file access: ✅ (within the folder you opened)
- Terminal commands: ✅ (Claude runs them after you approve)
- Internet access: ✅ (only when Claude asks for it and you allow it)

Get it: `claude.com/claude-code`. Requires a paid Anthropic plan (Pro ~$20/mo or higher).

### Claude Code — terminal / CLI

A program you run by typing `claude` in a terminal window (the black-or-white window where you type commands). The current folder you're in becomes the project folder. Power users like it; first-timers usually don't need it.

- Slash commands: ✅
- Local file access: ✅ (the folder you launched from)
- Terminal commands: ✅
- Setup: requires Node.js to be installed first

### Claude Code — inside your editor (VS Code, JetBrains, Cursor)

An extension that puts Claude Code into the editor you already use. Skip this until you've used the desktop app once and want it closer to your code.

### Regular Claude (claude.ai)

The chat window in your web browser at `claude.ai`. This is normal Claude — great for asking questions and getting written work — but it does **not** run on your computer, does **not** edit files on your disk, and does **not** support slash commands or skills.

The free Claude plan lives here. NewClauder and Claude Code are not on the free plan.

If you opened this tour expecting it to work in claude.ai: it won't fully. You need Claude Code.

### Claude in the iPhone or Android app

Same as claude.ai — a chat, no file access, no slash commands. Useful for quick questions on the go. Not where NewClauder lives.

---

## "Why doesn't this slash command work?"

- You're in claude.ai instead of Claude Code → switch to Claude Code.
- You typed the command but didn't press Enter → press Enter.
- You're in Claude Code but the command isn't installed → type `/help` and see what's available. If the command you wanted isn't there, it's a plugin or skill you haven't installed yet.

---

## "Where do shell commands run?"

When Claude runs a shell command (like `ls`, `git status`, `python script.py`), it runs **on your computer**, in **the folder you opened**, with **your user's full permissions**.

- Desktop app → in the folder you picked
- Terminal → in the folder you launched `claude` from
- Editor extension → in the project root of the editor

If you want Claude to work in a different folder, close this session and open a different one with that folder loaded. There's no `cd` mid-session that flips Claude's whole world.

---

## "How do I work on different projects?"

One folder per session. To switch:

- Desktop app: open a new window or pick a different folder
- Terminal: `cd /path/to/other/project` then run `claude` again
- Editor: open the other project in your editor; Claude follows

Each project can have its own `CLAUDE.md` — a little file in the folder that tells Claude about that specific project ("this is a Python codebase, never touch the `secrets/` directory, run tests with `make test`"). Newcomers don't need to write one on day one, but it's where the leverage shows up later.

---

## "How do I find the files Claude makes?"

Claude tells you the full path every time it writes a file. The path looks like:

- macOS: `/Users/yourname/Documents/something.md` or `~/Documents/something.md`
- Windows: `C:\Users\yourname\Documents\something.md` or `%USERPROFILE%\Documents\something.md`
- Linux: `/home/yourname/Documents/something.md` or `~/Documents/something.md`

To open it:

- **macOS:** in Terminal or in Claude Code, run `open /the/full/path` — it opens in your default editor.
- **Windows:** press `Win+E` (opens File Explorer), paste the path into the address bar, press Enter.
- **Linux:** `xdg-open /the/full/path`.

To read `.md` files comfortably (they're plain text but look prettier in a real reader): install one of these free apps:

- **Obsidian** — free, cross-platform, what most people use: `obsidian.md`
- **VS Code** — free, has a built-in Markdown preview (Ctrl/Cmd + Shift + V): `code.visualstudio.com`

You can also drag any `.md` file into a browser tab and it'll display as plain text — readable, just not pretty.
