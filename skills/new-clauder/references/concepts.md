# Concept reference — plain explanations

Deeper write-ups of each concept named in `SKILL.md`. Read the one you need before teaching it. Each entry is written to be readable aloud in under a minute, with an IT/InfoSec-flavored example so the audience sees themselves in it.

If a term in here is itself unfamiliar to the user, define it inline — never assume.

---

## Sessions

A session is one conversation with Claude Code — this chat, right here. It starts when you open it (running `claude` in the terminal, or opening the app), and ends when you exit. Everything inside it — your messages, Claude's responses, files read, commands run — is the "context" of that session.

**Why it matters:** Context is finite. When it gets long, Claude either compresses older parts automatically or you start fresh (`/clear`). A new session does not remember the previous one unless you wrote something down (a file, a memory entry, a `CLAUDE.md`).

**What breaks without this:** Users get confused when "Claude forgot." It didn't forget — it's a new conversation, or older context was summarized to make room.

**InfoSec example:** A long IR session where you've been parsing logs for two hours. Eventually the chat compacts. If you want anything to survive, write it to a file: "Save what we've learned so far to `incident-notes.md`."

---

## Tools

Tools are the verbs Claude can perform. The built-in ones include:

- Read a file
- Write or edit a file
- Run a shell command (Bash on Mac/Linux, PowerShell on Windows)
- Search files (Grep — find text in files; Glob — find files by name pattern)
- Fetch a URL (pulls one specific page)
- Search the web (general search)

Every tool call shows up in the chat. You see what Claude is about to do before it runs.

**Why it matters:** Claude without tools is a chatbot. Claude with tools changes files on your machine and runs real commands. That is the whole power — and the whole risk.

**The honest version of what tool execution means:** when you approve a Claude Code shell command, it runs with *your user's full permissions* on this machine — same as if you typed it yourself. Approving `rm -rf` deletes files. Approving "curl this URL and pipe it to bash" runs whatever that script says. There's no sandbox between Claude and your filesystem; the approval prompt *is* the safety layer. Read the proposed command before you say yes.

**InfoSec example:** "Run `netstat -an` and tell me which ports are listening" — Claude proposes the command, you approve, output comes back, Claude explains it. If instead it proposed something destructive based on misreading your intent, the approval gate is what catches it.

---

## Prompt injection (the new risk class)

A risk that's specific to AI agents and worth understanding before you let one near anything important.

**What it is:** Claude reads file contents and web pages as both *data* (information to think about) AND *instructions* (things to act on). A malicious file or webpage can include text that tries to redirect Claude into doing something you didn't ask for — install malware, exfiltrate data, run a destructive command, you name it. The technical name is *prompt injection*. It's the defining new threat class for agentic AI tools in 2026.

**The tell:** if Claude proposes an action that doesn't match what you originally asked for *after it just read a file or visited a URL*, that's the signature of an injection attempt. Stop and check before approving.

**Mitigations:**
- Use **plan mode** (`/permissions`) for first sessions and any session pointed at unfamiliar data — Claude can read and propose but can't act.
- Read every command before approving. Especially when Claude has just consumed untrusted input.
- Don't auto-approve everything just to move faster.
- For high-stakes work (production systems, sensitive data), don't run Claude Code there at all.

**InfoSec framing:** This maps to OWASP LLM01 (Prompt Injection) and OWASP Agentic AI Threats T1/T6 (indirect prompt injection via tool surfaces). It's not theoretical — it's been demonstrated repeatedly against production agentic systems.

---

## Permissions and modes

Claude asks before anything risky. Four behavior modes you'll see:

- **default** — ask before risky actions. Use this until you have a feel for the tool.
- **acceptEdits** — auto-approve file edits, still ask for shell commands. Good for focused coding sessions.
- **bypassPermissions** — auto-approve everything. Only ever use this in a throwaway environment (a fresh VM you can wipe). Never on a real work machine. Removes the prompt-injection mitigation entirely.
- **plan** — read-only mode. Claude can explore and propose but cannot change anything until you exit plan mode. **Recommended for your first session and any session pointed at unfamiliar data.**

Set the mode with `/permissions`.

Permanent allow/deny rules live in `settings.json` (a config file in `~/.claude/` on your machine or `.claude/` inside a project). You usually don't edit it by hand — Claude offers to add rules as you approve commands.

**InfoSec example:** Working in a folder of real production logs? Start in **plan mode**. Claude can read and analyze but can't accidentally modify or delete anything. Switch out only when you're ready to act.

**Plan mode is not a sandbox.** It's a review gate. It stops Claude from acting, but if you approve a bad plan, the bad plan runs. The control is the human reading the plan before approving — *you*.

---

## Folders and projects

Claude Code thinks about ONE folder at a time. That folder — and everything inside it — is what Claude can see and touch.

**To switch projects:** open a different folder.

- Desktop app → folder picker, open a different folder
- Terminal → `cd /path/to/other/project` then run `claude` again
- Editor extension → open the other project in your editor

There's no `cd` mid-session that flips Claude's whole world. One folder per session.

**Why it matters:** when you ask Claude to "read all files in this folder," it really means the folder you opened. Files outside that folder are invisible to it unless you give an explicit absolute path. This is the safety boundary — Claude can't accidentally rummage through your whole disk.

**Pro tip:** put a `CLAUDE.md` in each project folder. It teaches Claude about that specific project — conventions, gotchas, what not to touch. Claude reads it automatically at session start.

---

## Slash commands

Anything starting with `/` is a slash command — a shortcut that runs inside this chat.

Useful ones:
- `/help` — list everything
- `/clear` — wipe the conversation and start fresh
- `/permissions` — change permission mode (including plan mode)
- `/config` — settings
- `/login` — sign in when prompted
- `/exit` — quit
- `/<skill-name>` — invoke a skill by name (rarely needed; skills usually trigger automatically)

Slash commands are NOT shell commands. `/ls` does not list files. To run a shell command directly, type `!ls`.

Slash commands work in Claude Code (desktop, terminal, IDE extension). They do **not** work in claude.ai (the browser chat).

---

## Skills

A skill is a folder of instructions (and sometimes scripts) that teaches Claude how to handle a specific kind of task well. Every skill has a `SKILL.md` with a `name` and a `description`.

**How they trigger:** When you start a session, Claude reads all skill descriptions. When your prompt matches what a skill says it covers, Claude loads the full skill and follows it. You usually don't call skills by name — they fire from intent.

**Why they matter:** Specialization without bloat. A "phishing-triage" skill exists dormant until you paste an email header. A "soc2-evidence" skill stays out of the way until you mention an audit. You can install many; the right one shows up when needed.

**InfoSec example:** A "log-triage" skill that knows your SIEM's quirks, your IR runbook format, and your ticket template — fires whenever you start an investigation, stays silent otherwise.

**Trust note:** skills can include scripts that execute on your machine. Read every skill before installing it — the SKILL.md and any scripts under `scripts/`. NewClauder is just markdown, no scripts; not every skill is.

---

## Subagents (agents)

A subagent is a focused Claude that gets spawned inside your session to handle one job, with its own conversation. Examples: a code-reviewer agent, a research agent, a debug agent.

**Why they matter:** Some jobs would otherwise eat the whole context (read 50 files, run 20 searches in parallel, scan a giant repo). A subagent does that work in isolation and reports back with a summary. Your main chat stays clean.

**When to use:** Long research, parallel independent tasks, anything where the raw output is huge but the answer is small.

**InfoSec example:** "Spawn an agent that scans this folder of YARA rules for ones that overlap and report the duplicates." You get the report; the noise stays in the subagent.

---

## MCP servers

MCP (Model Context Protocol) is how Claude Code plugs into outside systems. An MCP server is a small program that exposes a set of tools to Claude — Notion, GitHub, Gmail, a database, a SIEM, a ticketing system.

Once configured, those tools show up alongside the built-in ones. Claude can `github-create-issue` the same way it can `Read` a file.

**Why they matter:** This is how Claude Code stops being just "a CLI tool on your laptop" and becomes connected to your real workflow.

**InfoSec examples:**
- GitHub MCP → read repos, search code, open issues, manage PRs
- Splunk / Elastic MCP → run searches from chat
- Notion / Confluence MCP → pull runbooks into context
- Jira / ServiceNow MCP → file and update tickets

**Each MCP server is its own trust decision.** Same way every plugin is. They add tool surface area to your Claude Code session — meaning more things prompt injection can try to hijack. Pick servers from trustworthy sources, audit their permissions, and don't connect everything just because you can.

---

## Hooks

Hooks are automated rules that fire on events. Examples:

- "Before any Bash command runs, log it to `~/audit.log`."
- "After Claude edits a file, run the formatter."
- "When the session starts, show `git status`."

Defined in `settings.json`. The harness — the program running Claude Code on your machine — runs them, not Claude itself. That makes hooks deterministic (they always fire, always the same way), which is the whole point.

**Why they matter:** Hooks turn Claude Code from a chat into a workflow. Power-user feature; skip on a first tour.

**InfoSec example:** A `PreToolUse` hook on Bash that logs every shell command Claude proposes to a tamper-evident log file — useful if you need an audit trail for who-did-what during an investigation.

---

## Memory and `CLAUDE.md`

Two related but distinct things:

- **`CLAUDE.md`** — a file in a project directory that teaches Claude about *that project*. Conventions, gotchas, what not to touch, where things live. Loaded automatically when a session starts in that folder.
- **Memory** — persistent notes about *you* that survive across sessions. Lives in `~/.claude/memory/`. Used for things like your role, preferences, recurring context.

**Why they matter:** Without these, every session starts from zero. With them, Claude shows up already knowing the project and the person.

**InfoSec example:** A `CLAUDE.md` in your IR notebook folder that says: "This folder contains incident data. Default to plan mode. Never `rm` anything. All findings get appended to `findings.md`, not overwritten." That preamble travels with the folder.

**NewClauder writes one memory note at the end of the tour** — your role, your terminal comfort, the first artifact you produced, and your chosen next step. No conversation content. You can read or delete it any time at `~/.claude/memory/user_claude_code_onboarding.md`.

---

## Bonus: "where does my data go?"

The most common silent question from an InfoSec audience. Brief answer:

- Your messages and the files Claude reads go to Anthropic for the model to process.
- By default, **Claude Code traffic is not used to train Anthropic's models.** (Verify on the Anthropic Trust Center — it's the source of truth.)
- Treat this like any cloud tool: do not paste secrets, credentials, customer data, PHI, or classified material without first checking your org's policy and any data-processing agreement.
- For high-sensitivity work, ask your security team whether your org has an enterprise agreement with zero-retention guarantees.
- The legal contract depends on your plan: Pro/Max/Team/Enterprise are under [Anthropic's Commercial Terms](https://www.anthropic.com/legal/commercial-terms); the free Claude (which can't run Claude Code anyway) is under [Consumer Terms](https://www.anthropic.com/legal/consumer-terms).

When in doubt, redact before pasting.
