# Session Patterns

Dialogue templates and scripts for each session mode. Referenced by SKILL.md during Phase 1 (Context Detection).

---

## Mode A: Smart (`/learn` with no arguments)

### Phase 1A: Context Detection

**Step 1 — Check session continuity**

Read `docs/learning-progress.json`. If `session_continuity.open_threads` is non-empty:

> "Last session we were exploring [concept]. You asked: '[their last question]' — at `[context file:line]`. Want to pick that up, or start fresh?"

Present via AskUserQuestion:
- A) Resume where we left off
- B) Start fresh from my latest changes
- C) Something else on my mind

**Step 2 — Detect recent changes**

```bash
git log --oneline -10 --format="%h %s" 2>/dev/null
echo "---"
git diff --stat HEAD~3 2>/dev/null || git diff --stat HEAD~1 2>/dev/null || git diff --stat 2>/dev/null
echo "---"
git diff HEAD~1 --no-color -U3 2>/dev/null | head -300
```

Map diff patterns to concepts using the concept extraction table in SKILL.md and `references/concept-map.md`. Identify 1-3 teachable concepts.

**Step 3 — Quick risk scan**

Grep changed files for patterns from `references/vibe-coder-risks.md` (top 5 highest-priority patterns only). If found, note them for weaving into the session naturally — don't lead with them.

**Step 4 — Opening**

> "You've been building. Let me see what you changed... [brief summary of changes]. I see [N concepts] worth exploring: [list]. Which one catches your eye?"

Or if risks were found:

> "I also noticed something in `[file]` that's worth understanding when you're ready."

---

## Mode B: Topic (`/learn <topic>`)

### Phase 1B: Direct Topic

**Step 1** — Parse `$ARGUMENTS` for the topic name.

**Step 2** — Check concept-map.md for the topic. If found, check prerequisites:
- If all prerequisites have confidence >= 3: proceed
- If a prerequisite is weak: "Before we dig into [topic], let's make sure you're solid on [prerequisite]. Quick check: [Socratic question about prerequisite]"

**Step 3** — Find where the topic appears in the codebase:

```bash
# Adapt the grep pattern to the topic
grep -rn "[topic-related pattern]" src/ 2>/dev/null | head -10
```

**Step 4** — Opening

> "[Topic] appears in your codebase at [locations]. Let's start with the one at `[file:line]`."

Then enter Phase 3 with a Level 1 open question about that specific code location.

---

## Mode C: Walkthrough (`/learn walkthrough`)

### Phase 1C: Action Tracing

**Step 1** — Ask for the entry point:

> "Name a user action in your app — something like 'user clicks Submit' or 'the dashboard loads.' I'll trace what happens from that moment through your entire codebase, layer by layer."

**Step 2** — Once they name an action, trace it through 5 layers. At each layer, ask before showing:

**Layer 1: UI Trigger**
- Find the component and event handler
- "What do you think happens when this [onClick/onSubmit/etc] fires? Before I show you — predict."

**Layer 2: Data Flow**
- Follow the function call chain
- "Where does the data go from here? Trace the path."

**Layer 3: Server / API**
- Find the API route handler
- "This hits your API. What does the server do with this request?"

**Layer 4: External / Database**
- If there's an external API call, DB query, or side effect
- "The data has reached [external system]. What happens now and how does your app know when it's done?"

**Layer 5: Response & Render**
- Trace the response back to the UI
- "The response comes back. How does it end up on screen? What triggers the update?"

**Step 3** — After tracing all layers, show the full journey:

```
User clicks "Start Call"
  → onClick handler in CallButton.tsx
    → fetch('/api/calls', { method: 'POST' })
      → route.ts processes request
        → Recall.ai API creates bot
      ← Returns bot ID
    ← setState updates call status
  → UI re-renders with "Call in progress"
```

> "That's the full journey. Which layer surprised you most?"

---

## Mode D: Audit (`/learn audit`)

### Phase 1D: Risk Scan

**Step 1** — Read `references/vibe-coder-risks.md` for the full risk catalog.

**Step 2** — Scan systematically (security first):

```bash
# Security risks
grep -rn "dangerouslySetInnerHTML\|eval(" src/ 2>/dev/null | head -5
grep -rn "innerHTML" src/ 2>/dev/null | head -5

# Input validation
grep -rn "req.body\|request.json\|req.query" src/ 2>/dev/null | head -10

# Secret exposure
grep -rn "process.env\|NEXT_PUBLIC_" src/ 2>/dev/null | head -10

# Error handling gaps
grep -rn "fetch(" src/ 2>/dev/null | head -10
# Then check which of these lack try/catch

# Auth checks
grep -rn "export.*GET\|export.*POST\|export.*PUT\|export.*DELETE" src/app/api/ 2>/dev/null | head -10
```

**Step 3** — For each finding, DO NOT explain the risk. Open with a Socratic question:

> "I see `[pattern]` at `[file:line]`. What do you think could go wrong here?"

**Step 4** — Prioritize findings and present a summary:

> "I found [N] patterns worth understanding. Let's start with the most important one."

Present top 3 as choices:
- A) [Security finding] — in `[file]`
- B) [Data integrity finding] — in `[file]`
- C) [Reliability finding] — in `[file]`

---

## Mode E: Recap (`/learn recap`)

### Phase 1E: Knowledge Report

**Step 1** — Read the full `docs/learning-progress.json`.

**Step 2** — Generate structured report:

```
YOUR LEARNING JOURNEY
══════════════════════════════════════════════
Started:         [first_session date]
Sessions:        [count]
Concepts:        [explored] / [total in codebase]

STRONG (confidence 4-5):
  ✓ [concept] — Bloom's: [level]
  ✓ [concept] — Bloom's: [level]

GROWING (confidence 2-3):
  → [concept] — [what's needed to advance]
  → [concept] — [what's needed to advance]

NOT YET EXPLORED:
  ○ [concept] — appears in [file], unlocks [downstream]
  ○ [concept] — appears in [file], unlocks [downstream]

DUE FOR REVIEW:
  ⟳ [concept] — last reviewed [date], due [date]

RECENT SESSIONS:
  [date]: [mode] — explored [concepts]
  [date]: [mode] — explored [concepts]
══════════════════════════════════════════════
```

**Step 3** — Offer choices:

> A) Review a concept that needs work
> B) Learn something new
> C) I just wanted the summary

---

## Mode F: Debug-Learn (`/learn why`)

### Phase 1F: Bug-Driven Learning

**Step 1** — Ask for the problem:

> "What went wrong? You can:
> - Paste an error message
> - Describe unexpected behavior
> - Point to code that confuses you"

**Step 2** — Identify the root concept:

| Error type | Likely concept |
|---|---|
| TypeError: Cannot read property of undefined/null | null vs undefined, optional chaining |
| Unhandled Promise rejection | async error handling, try/catch |
| CORS error | Cross-origin security, API architecture |
| 404 on API route | File-based routing, HTTP methods |
| State not updating | React state, async state updates |
| Hydration mismatch | Server vs client rendering |
| Module not found | Import/export, module resolution |

**Step 3** — Do NOT fix the bug. Teach the concept:

> "This error is rooted in [concept]. Instead of fixing it for you, let's learn about [concept] — then you'll be able to fix this yourself and recognize it next time."

**Step 4** — After teaching, return to the bug:

> "Now that you understand [concept], look at the error again. What's causing it? How would you fix it?"

If they can fix it themselves → major confidence boost, note in progress.
If they need help → guide Socratically, not by giving the answer.

---

## Mode G: Review (`/learn review`)

### Phase 1G: Spaced Repetition

**Step 1** — Select concepts for review:

1. Primary: concepts where `spaced_repetition.next_review <= today`
2. Fallback: 3 lowest-confidence concepts
3. Exclude: confidence >= 5 AND last_reviewed within 30 days

**Step 2** — For each concept, find current usage in codebase:

```bash
grep -rn "[concept-related pattern]" src/ 2>/dev/null | head -5
```

**Step 3** — Ground the review in current code (not abstract recall):

> "You learned about [concept] [N days ago]. I see it at `[file:line]`. Without reading the surrounding code — what do you think this [specific usage] is doing?"

**Step 4** — Rate quality (0-5) based on response and update spaced repetition:

| Quality | Response | Update |
|---|---|---|
| 5 | Perfect, could teach it | Increase interval significantly |
| 4 | Correct with minor hesitation | Increase interval |
| 3 | Correct with effort | Maintain interval |
| 2 | Mostly wrong but showed some recall | Decrease interval |
| 1 | No recall, needed full re-explanation | Reset to shortest interval |
| 0 | Complete blank | Reset, flag for intensive review |

---

## Mode H: Roadmap (`/learn next`)

### Phase 1H: Next Concept Selection

**Step 1** — Scan codebase for all concept patterns:

```bash
# Sample of what to scan for
grep -rn "async\|await\|Promise" src/ 2>/dev/null | wc -l
grep -rn "useState\|useEffect\|useContext" src/ 2>/dev/null | wc -l
grep -rn "fetch(\|axios" src/ 2>/dev/null | wc -l
grep -rn "interface \|type " src/ 2>/dev/null | wc -l
# ... etc for each concept in concept-map.md
```

**Step 2** — Cross-reference with progress file. Find concepts that:
- Appear in the codebase but aren't in the progress file
- Have all prerequisites met (confidence >= 3)

**Step 3** — Calculate usefulness score per concept:
- How many downstream concepts does it unlock?
- How frequently does it appear in the codebase?
- How soon will they encounter it in their work?

**Step 4** — Present top 3 as motivated choices:

> "Here are three concepts in your codebase you haven't explored yet:"
>
> A) **Error handling** — appears in 12 files. Unlocks: API reliability, graceful degradation, production readiness
> B) **TypeScript types** — appears in 8 files. Unlocks: generics, validation, API contracts
> C) **useEffect** — appears in 5 files. Unlocks: data fetching, cleanup patterns, lifecycle control
>
> "Which one are you most curious about?"

---

## Session Exit Criteria

End the session gracefully when:

1. **3-4 concepts covered** — "We've explored a lot. Want to keep going or wrap up?"
2. **20+ minutes estimated** — "This has been a solid session. Let's recap."
3. **Learner signals fatigue** — Short answers, "yeah," "ok," "sure" → offer wrap-up
4. **Learner says they're done** — Immediately move to recap

### Session Recap Template

> "Here's what we covered today:
>
> **[Concept 1]** — You went from [before] to [after]. Confidence: [N/5].
> **[Concept 2]** — [Summary of what they learned]. Confidence: [N/5].
>
> **Next time**: [What to explore or review next]
> **Open thread**: [Any unfinished question, saved for next session]"
