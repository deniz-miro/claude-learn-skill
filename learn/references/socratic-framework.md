# Socratic Teaching Framework

Reference material for the `/learn` skill's dialogue engine. Read this file at the start of each session.

## The Graduated Hint Ladder

Five levels of scaffolding. Always start at Level 1. Only advance one level at a time. Never skip.

### Level 1: Open Question

Ask what they think with zero framing. Point to specific code but don't hint at the answer.

**Entry**: Always. This is the default opening.

**Templates**:
- "Looking at `[file:line]`, what do you think is happening here?"
- "I see `[code pattern]` in your codebase. What's your read on what this does?"
- "Before I say anything — what's your instinct about why `[code]` is here?"

**Exit to Level 2**: Learner says "I have no idea," gives no response, or their answer shows no foothold at all.

**Do NOT advance if**: They give a partially correct answer. Build on what's right instead.

### Level 2: Narrowing Question

Connect the concept to something from their professional background or a concept they already know.

**Entry**: Only after Level 1 failed to produce a foothold.

**Templates**:
- "This is related to [broader category]. In your [their domain] work, what's similar to [concept]?"
- "Think about [known concept they scored 3+ on]. This is in the same family. What connection do you see?"
- "Let me narrow the scope — this part specifically: `[smaller code snippet]`. Does this piece make more sense on its own?"

**Exit to Level 3**: The analogy or narrowing doesn't connect. They're still stuck.

### Level 3: Leading Question

Reveal a partial fact and ask them to reason from it. The fact should be a stepping stone, not the answer.

**Entry**: Only after Level 2 failed to produce a connection.

**Templates**:
- "What if I told you that [partial fact about the mechanism]? Given that, what do you think happens when `[their code]` runs?"
- "Here's a clue: [concept] exists because [reason]. Knowing that, why might this code use it here?"
- "Consider this: without `[keyword/pattern]`, this code would [observable consequence]. What does that tell you about what it does?"

**Exit to Level 4**: They still can't make the connection after reasoning from the partial fact.

### Level 4: Partial Answer

Give a short, plain-language explanation of the mechanism (1-2 sentences max), then immediately ask them to apply it to their specific code.

**Entry**: Only after Level 3 failed, or after 2 attempts at Level 3 that didn't land.

**Templates**:
- "Here's what's happening: [mechanism in 1-2 sentences]. Now, looking at your code at `[file:line]` — what does that mean for this specific line?"
- "[Brief explanation]. Given that, what would change if you removed this line?"
- "The short version: [mechanism]. With that in mind, can you trace what happens when `[function/route]` gets called?"

**Exit to Level 5**: They explicitly ask "just tell me" or cannot engage after 2 attempts at Level 4.

### Level 5: Full Explanation

Give a complete, plain-language explanation. Max 4 sentences. Always end with a follow-up question — never end on an explanation.

**Entry**: Only when the learner explicitly requests it, or after Level 4 fails twice.

**Templates**:
- "[Complete explanation, 4 sentences max]. Does that click? In your own words, what does `[their specific code]` do?"
- "[Explanation]. Now here's a check — if I changed `[X]` to `[Y]` in this code, what would be different?"

**Post-Level-5 rule**: Flag this concept in the progress file with `"hint_level_reached": 5`. Schedule it for early review. The learner got the explanation but hasn't demonstrated understanding yet.

---

## The 6 Socratic Question Types

Use a mix of these throughout the dialogue. Don't use the same type twice in a row.

### 1. Clarification Questions

Push them to be precise about what they think they know.

- "When you say 'it sends the data,' what exactly happens at the network level?"
- "You said this 'updates the state.' What does 'state' mean in this context?"
- "Can you be more specific about which part 'handles the error'?"

**Use when**: Their answer is vague or uses hand-wavy language. Forces specificity.

### 2. Assumption Questions

Surface hidden assumptions they're making about how the code works.

- "You're assuming this function always succeeds. What happens when it doesn't?"
- "What if this data is empty? Or null? Does your mental model still hold?"
- "You said this runs 'first.' Are you sure about the order? What controls that?"

**Use when**: Their answer is correct for the happy path but misses edge cases.

### 3. Evidence Questions

Ask them to point to the specific code that supports their claim.

- "What in the code tells you this is asynchronous? Can you point to the exact syntax?"
- "Where do you see the data flowing from the API to the component?"
- "You said this is a POST request. What line tells you that?"

**Use when**: Their answer might be a guess. Evidence questions separate understanding from pattern-matching.

### 4. Alternative Questions

Ask them to consider other approaches and compare.

- "This uses `useState`. What other ways could this component remember things?"
- "You could also write this with a `for` loop. What's different about `.map()`?"
- "Why a webhook here instead of polling? What's the tradeoff?"

**Use when**: They understand what the code does but not why this approach was chosen. Builds architectural thinking.

### 5. Implication Questions

Ask them to trace consequences — what happens if X changes, breaks, or is removed?

- "If you delete this `await`, what happens to the rest of the function?"
- "What does the user see if this API call takes 10 seconds?"
- "If this environment variable is missing, what breaks?"

**Use when**: They understand the mechanism but not its importance or fragility. Builds production awareness.

### 6. Meta Questions

Ask them to reflect on their own thinking process.

- "What pattern are you recognizing here? Where have you seen something like this before?"
- "What was the hardest part of that to understand? What made it click?"
- "How would you explain this to someone on your team?"
- "What question would you ask next if you were trying to learn this?"

**Use when**: After a concept lands, to consolidate understanding and build metacognition. Also powerful as session wrap-up.

---

## Bloom's Taxonomy for Code

Detect the learner's current level per concept and teach at that level, then push one level up.

### Level 1: Remember
- **Indicator**: Can recall syntax but not meaning. "await makes it... wait?"
- **Question style**: "What does this keyword do?" / "What's the name for this pattern?"
- **Goal**: Recognition and recall of terminology

### Level 2: Understand
- **Indicator**: Can explain what code does in their own words. "It pauses the function until the data comes back."
- **Question style**: "Why is this here?" / "What problem does this solve?"
- **Goal**: Explain the concept, not just name it

### Level 3: Apply
- **Indicator**: Can use the pattern in a new context. "I'd add await here too because this is also an API call."
- **Question style**: "If you needed to do [related task], how would you start?"
- **Goal**: Transfer understanding to new situations

### Level 4: Analyze
- **Indicator**: Can compare approaches and identify relationships. "This is like the other API call but with error handling."
- **Question style**: "What's the relationship between this and [other pattern]?"
- **Goal**: Break down complex code into parts and see connections

### Level 5: Evaluate
- **Indicator**: Can judge quality and make tradeoff decisions. "Polling works but a webhook would be more efficient here."
- **Question style**: "Is this the right approach? What are the tradeoffs?"
- **Goal**: Assess design decisions critically

### Level 6: Create
- **Indicator**: Can design a solution from scratch. "I'd structure the API with three routes..."
- **Question style**: "How would you design this from scratch?"
- **Goal**: Synthesize knowledge into new solutions

### Detection heuristic

Listen to HOW they answer, not just whether they're correct:
- Recites syntax → Remember
- Explains in own words → Understand
- Applies to new scenario → Apply
- Compares/contrasts approaches → Analyze
- Judges quality with reasoning → Evaluate
- Proposes original design → Create

Always teach at their current level and offer a question at one level up. Don't jump two levels.

---

## Diagnostic MCQ Design Rules

Use MCQs sparingly — open questions are preferred. When you do use them:

### Rule 1: Every wrong answer maps to a specific misconception

Don't include random wrong answers. Each option should represent a real mental model that a learner might have. When they pick a wrong answer, you learn what they misunderstand.

### Rule 2: Diagnose, don't judge

After any answer (right or wrong), explain what the answer tells you about their thinking:
- "You picked A — that tells me you're thinking of it like [mental model]. The key difference is..."
- "That's right. What made you pick that over C? Understanding why you rejected C is as valuable as getting the right answer."

### Rule 3: The correct answer should not stand out

- Never make it the longest or most detailed option
- Never use "all of the above" or "none of the above"
- Never include an obviously absurd option
- Every option should be plausible to someone who doesn't yet know the concept

### Rule 4: Follow every MCQ with an open question

An MCQ tests recognition. Follow it with: "Now explain it in your own words — why does `[their code]` work this way?"

### Common misconception templates for coding concepts

**async/await**:
- "Pauses the whole program" → single-threaded misconception
- "Makes things run in parallel" → concurrency vs parallelism confusion
- "Is a timeout mechanism" → conflating await with setTimeout

**API routes**:
- "The browser runs the API code" → client/server confusion
- "GET and POST do the same thing" → HTTP method conflation
- "The API sends HTML to the browser" → REST vs server-rendering confusion

**useState**:
- "The variable changes immediately" → synchronous state misconception
- "State persists between page refreshes" → state vs storage confusion
- "Changing state rerenders the whole app" → component-level rerender misunderstanding

**TypeScript types**:
- "Types make the code run differently" → types as runtime values misconception
- "Types catch all bugs" → type safety overconfidence
- "You can skip types and add them later" → technical debt blind spot

---

## Answer Leak Prevention Checklist

Before presenting any multi-choice options, check:

1. **Does any option assume comprehension?**
   - Bad: "Show me where else this pattern appears" (assumes they understand the pattern)
   - Good: "I want to see more examples of this"

2. **Does any option contain the answer?**
   - Bad: "What would break if I removed the await?" (reveals that removing await breaks something)
   - Good: "What happens if I change this line?"

3. **Does the option structure reveal the correct answer?**
   - Bad: One option is much more detailed or specific than others
   - Good: All options are roughly the same length and specificity

4. **Do the options actually branch?**
   - Bad: All options lead to the same explanation, just framed differently
   - Good: A) leads to mechanism deep-dive, B) leads to analogy, C) leads to experimentation, D) moves on
