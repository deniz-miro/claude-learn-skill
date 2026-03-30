# /learn — Socratic Coding Tutor for Vibe Coders

A Claude Code skill that teaches programming through Socratic dialogue. Built for vibe coders — people who build real apps with AI but want to understand what they built, not treat it as a black box.

## The Problem

Everyone is vibe coding. AI writes the code, you ship the app. But when something breaks, you're stuck. You don't know what an API route does, why `await` matters, or what happens when your fetch call fails. This skill fixes that — it teaches you the foundations by asking questions about your own code, not by lecturing.

## What it does

- **Socratic method**: Never explains first. Asks what you think, builds on your answer, follows your curiosity
- **5-level hint ladder**: Starts with open questions, only gives answers as a last resort — and always ends with a follow-up question
- **Context-aware**: Reads git diffs, scans your codebase, traces user actions through your code
- **Adapts to you**: Learns your background (design, PM, data, research) and tailors analogies to your world
- **Tracks progress**: Spaced repetition, Bloom's Taxonomy levels, misconception tracking, session continuity
- **8 session modes**: From quick diff-based lessons to full codebase walkthroughs and security audits

## Install

```bash
# Clone this repo
git clone https://github.com/deniz-miro/claude-learn-skill.git

# Copy the skill into your Claude Code skills
cp -r claude-learn-skill/learn ~/.claude/skills/learn
```

That's it. The skill is now available as `/learn` in any Claude Code session.

## Usage

| Command | What it does |
|---|---|
| `/learn` | Smart mode: detect recent changes + resume open threads + risk scan |
| `/learn <topic>` | Teach a specific concept (e.g., `/learn async/await`) |
| `/learn walkthrough` | Trace a user action through your codebase layer by layer |
| `/learn audit` | Scan your code for risky patterns vibe coders often miss |
| `/learn recap` | "State of your knowledge" report across all sessions |
| `/learn why` | Learn the concept behind a bug, then fix it yourself |
| `/learn review` | Spaced repetition review of concepts due for practice |
| `/learn next` | Suggest the highest-value next concept with "this unlocks..." |

## How it works

### First session
Asks 3 questions: your background, your coding experience, and whether AI helped write your code. Stores your profile in `docs/learning-progress.json`. From then on, everything is personalized.

### Teaching approach
Every exchange follows the **Think-Articulate-Reflect** loop:
1. Asks what you think (grounded in your actual code, not abstract examples)
2. Builds on your answer — right, wrong, or "I have no idea"
3. Asks you to reflect on how your understanding changed

Questions never leak the answer. Multiple-choice options are learning directions ("go deeper," "connect to what I know," "try applying it"), not quiz answers.

### The hint ladder
```
Level 1: Open question     → "What do you think this does?"
Level 2: Narrowing         → "It's related to [thing you know]..."
Level 3: Leading           → "What if I told you [partial fact]..."
Level 4: Partial answer    → "[1-2 sentences]. Now what happens at your code?"
Level 5: Full explanation  → "[4 sentences max]. [Follow-up question]."
```
Never skips levels. Even at Level 5, ends with a question.

### Progress tracking
- **Confidence scale** (1-5): Assessed through dialogue, not self-reporting
- **Bloom's Taxonomy**: Tracks whether you can remember, understand, apply, analyze, evaluate, or create
- **Spaced repetition** (SM-2): Automatically schedules review of concepts that might fade
- **Session continuity**: Saves open threads so you can pick up where you left off
- **Misconception tracking**: Notes specific wrong mental models and revisits them

## Who it's for

Anyone who builds with AI and wants to understand what they built:
- **Designers** picking up code through prototyping
- **PMs** building internal tools with AI assistance
- **Researchers** automating their workflows
- **Founders** shipping MVPs with Claude/Cursor/Copilot
- **Career switchers** learning development by doing

The skill adapts analogies to your background. A designer gets visual/spatial metaphors. A PM gets process/workflow analogies. A data analyst gets spreadsheet/query comparisons.

## Concept coverage

~50 concepts across 5 tiers:

| Tier | Topics | Key concepts |
|---|---|---|
| 1. Foundations | HTML, components, routing | Props, imports/exports, file-based routing, server vs client |
| 2. Interactivity | State, effects, styling | useState, useEffect, Tailwind, CSS tokens, build process |
| 3. Data Flow | HTTP, APIs, async | Fetch, error handling, webhooks, polling, CORS, serialization |
| 4. Architecture | Patterns, types, data | State machines, generics, validation, databases, middleware |
| 5. Security & Production | Risk awareness | Input validation, XSS, auth, secret management, error boundaries |

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
    ├── concept-map.md                50 concepts with multi-background analogies + dependency graph
    ├── socratic-framework.md         Hint ladder, question types, MCQ design, Bloom's detection
    ├── session-patterns.md           Per-mode dialogue templates and scripts
    └── vibe-coder-risks.md           Risky patterns catalog with grep signatures + Socratic openers
```

## Philosophy

> "The AI never spontaneously suggests sophisticated concepts. You need domain knowledge to guide it."

Understanding is the bottleneck skill for vibe coders. Not generating code — understanding it. This skill exists so that every building session can also be a learning session.

## License

MIT — Deniz Kartepe
