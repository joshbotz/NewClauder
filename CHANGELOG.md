# Changelog

## 2.0.0 — 2026-05-16

**Breaking change:** plugin renamed from `claude-code-tour` to NewClauder. New install commands required for anyone running v1.x.

**Renames:**
- Display name: Claude Code Tour → **NewClauder**
- Technical identifier: `claude-code-tour` → `new-clauder` (kebab-case convention)
- GitHub repo: `botz-pillar/claude-code-tour` → **`botz-pillar/NewClauder`**
- Skill directory: `skills/claude-code-tour/` → `skills/new-clauder/`

**New install commands:**
```
/plugin marketplace add botz-pillar/NewClauder
/plugin install new-clauder@new-clauder
```

Anyone with v1.x installed should uninstall (`/plugin uninstall claude-code-tour`) and reinstall under the new name.

**Reference files refreshed for v1.2 safety/voice consistency:**
- `references/concepts.md` — added a dedicated **Prompt Injection** section as the defining new risk class of agentic AI tools, with the "if Claude proposes something that doesn't match what you asked for after reading untrusted input, that's the signature of an injection attempt" detection cue. Expanded the **Tools** section with tool-execution-honesty framing (commands run with the user's full permissions). Promoted **Plan mode** to "recommended for first session and unfamiliar data." Added a **Folders and projects** section. Added a **Trust note** to Skills covering pre-install script audit. Added MCP-server supply-chain framing. Updated **Where does my data go** with the Commercial-vs-Consumer Terms split.
- `references/install.md` — added the pre-install checks (work-laptop/sensitive-data/MDM/DLP/EDR), the "Claude Code requires a paid plan" disclosure with Pro/Max/API options, the "try free claude.ai first" sidebar, Windows/Linux install paths for the terminal version, the `/help` confirmation test.
- `references/where-am-i.md` — minor consistency pass for NewClauder brand and the cost split (free Claude ≠ Claude Code).
- `references/starter-prompts.md` — added plan-mode safety hint to SOC prompts that handle potentially untrusted content (phishing emails, decoded payloads).

**Metadata:**
- `plugin.json` description tightened to match the NewClauder voice
- `marketplace.json` description tightened similarly

Anyone reviewing the plugin source for security before installing will find every file in the repo is plain markdown — no scripts, no network calls. Standard practice: read every plugin before you install it.



## 1.2.0 — 2026-05-16

Significant README rewrite incorporating two rounds of battle-test review.

**Safety & honesty (the big lift):**
- New section 4 "What this means for your machine" — explicit tool-execution honesty (approved commands run with user's full privileges), prompt injection acknowledgment with a detection cue, plan mode promoted as the first-session recommendation
- New section 3 "What you'll actually be able to do" — concrete role-anchored capabilities listed before the cost reveal, plus a "use synthetic data for the tour exercise" callout
- Section 8 expanded with "read every plugin before you install it" as a standing security habit, not a one-off note
- Section 2 work-laptop check expanded with MDM/DLP/EDR realities and a fourth "personal machine with work data" branch
- Terms split clarified: Commercial vs Consumer Terms, with explicit links

**Cost honesty:**
- Pricing disclosure (section 5) moved BEFORE the claude.ai signup step (was: reveal after signup — bait-and-switch)
- Pricing simplified to "Start with Pro at ~$20/mo, here's why" recommendation; Max tiered with honest "starting around $100/mo" hedge
- New "want to feel out the vibe first?" sidebar pointing to free claude.ai before subscribing
- Direct statement: free Claude plan does NOT include Claude Code

**Sequencing & cognitive load:**
- Install Claude Code (section 7) now has a confidence checkpoint with the `/help` decisive test before the plugin install
- Path A install warns about admin-rights / managed-machine credential prompts
- Path B install now covers macOS / Windows / Linux (was Mac-only)
- Trigger phrases cut from 5 to 3 most distinctive
- "Where am I?" reference (section 10) repositioned with forward references from sections 4 and 7
- Concept list (section 11) now has one-line plain-English gloss per term

**Onboarding for absolute beginners:**
- New subtitle: "Get from install to your first useful Claude Code session in about 15 minutes — even if you've never touched a terminal."
- Helpful-links section (13) curated to 9 highest-leverage items across 5 categories
- Memory-note structure shown literally with deletion path
- Feedback section names response expectation ("read every message, reply within a week")
- "Doesn't phone home" replaced with precise "makes no network calls of its own"
- Removed the "looking dumb / no shame button" framing; replaced with operational fears ("worried about pasting the wrong thing")

## 1.1.0 — 2026-05-16

README expansion for zero-knowledge beginners.

- Anthropic account signup, app install, and plugin install walked end-to-end
- Each install command in its own visually-separated block
- Sidebar explaining Claude vs. Claude Code
- "Helpful links for beginners" section
- "Magic phrase for when you're stuck" callout
- Table of contents

## 1.0.0 — 2026-05-16

Initial public release.

- Role-aware onboarding tour for IT/InfoSec professionals new to Claude Code
- Teaches concepts in-flight during real work; ends with one concrete artifact
- Privacy/safety story baked into Step 2
- Six "in-flight teaching" moments tied to natural beats during real work
- 35+ role-conditioned example pitches
- Reference files: where-am-i.md, starter-prompts.md
- Creative-sparks behavior, file-output narration, memory write, skip-the-tour escape hatch
- README written for someone who has never used GitHub
