# /learn — Socratic Tutor for Vibe Coders

A Claude Code skill that teaches through Socratic dialogue, not lectures. Built for anyone who works with AI and wants to understand what they built — whether that's code, configurations, workflows, or structured data.

## The Problem

Everyone is vibe coding. AI writes the code, you ship the app. But when something breaks, you're stuck. You don't know what an API route does, why `await` matters, or what happens when your fetch call fails.

And it's not just code. PMs use AI to generate configs they don't understand. Designers prototype with frameworks they can't debug. Researchers build automation pipelines that are black boxes. The pattern is the same: AI does the work, you lose control.

This skill fixes that. It teaches you the foundations by asking questions about your own work — not by lecturing.

## What it does

- **Socratic method**: Never explains first. Asks what you think, builds on your answer, follows your curiosity
- **5-level hint ladder**: Starts with open questions, only gives answers as a last resort — and always ends with a follow-up question
- **Context cascade**: Adapts to whatever you're working with — git diffs, codebases, config files, markdown docs, or just a conversation. Never dead-ends.
- **Adapts to you**: Learns your background (design, PM, data, research) and tailors analogies to your world
- **Frustration-aware**: Detects when you're stuck and changes approach instead of pushing harder
- **Teach mode**: When you're confident, flip the roles — you teach, the skill plays dumb and asks naive questions
- **Progress tracking**: Spaced repetition, session continuity, shareable learning artifacts
- **Delight**: Celebrates real growth moments, remembers your journey across sessions

## Install

```bash
git clone https://github.com/deniz-miro/claude-learn-skill.git
cp -r claude-learn-skill/learn ~/.claude/skills/learn
```

That's it. The skill is now available as `/learn` in any Claude Code session.

## Where your progress lives

By default, `~/.claude/learning-progress.json`. One file, persists across every project and every session on your machine. Switching repos, cloning a fresh checkout, or migrating a codebase doesn't reset your progress — because the file lives next to your Claude config, not inside any single project.

**Override:** set `$LEARN_PROGRESS_PATH` to point elsewhere if you want a custom location (e.g. a synced cloud folder, or a sandbox-wide path that spans multiple project directories).

**Migrating from older versions:** if you used the skill before May 2026, your progress probably lives at `<repo>/docs/learning-progress.json`. The skill auto-detects this on first run, copies it to `~/.claude/learning-progress.json`, and tells you in the session recap. The old file is kept as a backup; safe to delete once you've confirmed the new location works.

## Heads-up: name collision with gstack

If you also use gstack, it ships its own `/learn` skill (a project-learnings manager — different concept entirely). Two skills, same handle. Pick one to rename:

- **Keep this one as `/learn`:** uninstall or rename gstack's `/learn`.
- **Rename this one:** `mv ~/.claude/skills/learn ~/.claude/skills/socratic_tutor`. Same skill, different invocation handle (`/socratic_tutor`).

If you've already hit the collision and the skills are tangled, the recovery snippet that re-installs this skill cleanly:

```bash
curl -sL "https://raw.githubusercontent.com/deniz-miro/claude-learn-skill/main/learn/SKILL.md" \
  -o ~/.claude/skills/socratic_tutor/SKILL.md
```

## Usage

**One command: `/learn`.** It figures out what to do based on your context.

The skill runs a detection cascade to find the best entry point for learning:

1. **Recent code changes?** Teaches from your git diff — the concepts you just used
2. **Codebase but no changes?** Explores what's already there — walkthrough, audit, or concept deep-dive
3. **Config, YAML, JSON, markdown files?** Teaches structured data concepts, schemas, configuration patterns
4. **No files at all?** Starts a conversation — "What have you been working on?" and teaches from there

You don't need to think about modes. Just type `/learn` after a building session, when you're stuck, or when you're curious about something.

You can also say `/learn <topic>` to learn something specific (e.g., `/learn async/await`, `/learn git branching`, `/learn API design`).

<details>
<summary>Advanced: override modes for power users</summary>

| Command | What it forces |
|---|---|
| `/learn walkthrough` | Trace a user action through your codebase layer by layer |
| `/learn audit` | Scan your code for risky patterns vibe coders often miss |
| `/learn recap` | "State of your knowledge" report across all sessions |
| `/learn why` | Learn the concept behind a bug, then fix it yourself |
| `/learn review` | Spaced repetition review of concepts due for practice |
| `/learn next` | Suggest the highest-value next concept with "this unlocks..." |

</details>

## How it works

### First session
Asks one question about your background, then immediately teaches you something from your project. Remaining profile questions are woven into the conversation naturally. No registration forms — you get value in the first 30 seconds.

Also asks: "What's the one thing you most wish you understood?" — your answer seeds the first real topic.

### Teaching approach
Every exchange follows the **Think-Articulate-Reflect** loop:
1. Asks what you think (grounded in your actual work, not abstract examples)
2. Builds on your answer — right, wrong, or "I have no idea"
3. Asks you to reflect on how your understanding changed

Questions never leak the answer. Choices are learning directions ("deeper," "connect," "try it," "next"), not quiz answers.

### When you're stuck
The skill detects frustration — repeated "I don't know," short answers, or you saying you're confused. Instead of pushing harder, it:
- Validates the struggle: "This trips up experienced engineers too"
- Changes approach entirely: new angle, make it concrete, zoom out, or park it for later
- Never makes you feel dumb for finding something hard

### When you're confident
At higher confidence levels, the skill offers **teach mode**: you explain the concept, the skill plays a curious beginner and asks naive questions. If you can teach it, you own it.

### Session endings
Every session ends with three things:
1. **What you now know** — stated as a capability, not a grade
2. **One thing to try** — a concrete action for your next session
3. **Share it** (optional) — save to a learning log or draft a Slack TIL post

No scores. No taxonomy labels. You should feel accomplished, not measured.

### The hint ladder
```
Level 1: Open question     → "What do you think this does?"
Level 2: Narrowing         → "It's related to [thing you know]..."
Level 3: Leading           → "What if I told you [partial fact]..."
Level 4: Partial answer    → "[1-2 sentences]. Now what happens at your code?"
Level 5: Full explanation  → "[4 sentences max]. [Follow-up question]."
```
Never skips levels. Even at Level 5, ends with a question.

## Who it's for

Anyone who works with AI and wants to understand what they're building:

- **Designers** prototyping with code frameworks they're still learning
- **PMs** building internal tools or automations with AI assistance
- **Researchers** creating data pipelines, analysis scripts, or automation workflows
- **Founders** shipping MVPs with Claude, Cursor, or Copilot
- **Career switchers** learning development by doing
- **Anyone** who uses AI to generate things they don't fully understand yet

The skill adapts to your background. A designer gets visual/spatial metaphors. A PM gets process/workflow analogies. A data analyst gets spreadsheet/query comparisons. A researcher gets study-design analogies.

## Concept coverage

~70 concepts across multiple domains:

**JavaScript / TypeScript / React** — Components, state, effects, routing, async/await, API routes, TypeScript types, testing, styling, modules

**Python** — Functions, classes, decorators, context managers, comprehensions, type hints, virtual environments, pandas, package management

**General programming** — Conditionals, loops, variables, functions, file I/O, git, Docker, CI/CD, SQL, configuration files

**Security & Production** — Input validation, XSS, auth, secret management, error boundaries, rate limiting

Each concept tracks:
- Code prerequisites (what you need to have seen)
- Mental model prerequisites (what you need to understand conceptually)
- What it unlocks (forward motivation)
- Common misconceptions (what vibe coders often get wrong)

## File structure

```
learn/
├── SKILL.md                          Core skill: workflow, dialogue rules, progress schema
└── references/
    ├── concept-map.md                70 concepts with multi-background analogies + dependency graph
    ├── socratic-framework.md         Hint ladder, question types, MCQ design, Bloom's detection
    ├── session-patterns.md           Per-mode dialogue templates and scripts
    └── vibe-coder-risks.md           Risky patterns catalog with grep signatures + Socratic openers
```

Reference files are lazy-loaded — only read when the current session actually needs them, keeping the context window focused on your work.

## Philosophy

> "The AI never spontaneously suggests sophisticated concepts. You need domain knowledge to guide it."

Understanding is the bottleneck skill for anyone working with AI. Not generating — understanding. This skill exists so that every working session can also be a learning session.

## License

MIT — Deniz Kartepe
