# Vibe Coder Risk Patterns

Risky patterns commonly found in AI-generated code that vibe coders accept without understanding. Used by `/learn audit` mode and for weaving risk awareness into regular teaching sessions.

Each entry has: the grep pattern to detect it, the risk it creates, the misconception it reveals, and a Socratic opening question.

## Priority 1: Security

### Unvalidated API Input

**Grep**: `req.body` or `request.json()` used without validation
```bash
grep -rn "req\.body\|request\.json\(\)\|req\.query" src/app/api/ 2>/dev/null
```
**Risk**: Accepts any data — malicious payloads, wrong types, missing fields
**Misconception**: "The frontend sends the right data, so the backend doesn't need to check"
**Socratic opener**: "Your API at `[file:line]` accepts whatever data comes in. What could someone send that you didn't expect?"

### Secrets in Client Code

**Grep**: Non-public env vars used in client components, or hardcoded keys
```bash
grep -rn "process\.env\." src/ --include="*.tsx" --include="*.jsx" 2>/dev/null | grep -v "NEXT_PUBLIC_"
grep -rn "sk-\|api_key\|secret\|password\|token" src/ 2>/dev/null | grep -v node_modules | grep -v ".env"
```
**Risk**: API keys visible in browser, anyone can steal them
**Misconception**: "Environment variables are always hidden" or "nobody will look at my code"
**Socratic opener**: "I see an API key being used at `[file:line]`. Who can see this value when your app runs in a browser?"

### XSS Vectors

**Grep**: `dangerouslySetInnerHTML`, `eval()`, `innerHTML`
```bash
grep -rn "dangerouslySetInnerHTML\|eval(\|\.innerHTML" src/ 2>/dev/null
```
**Risk**: Users (or attackers) can inject JavaScript into your page
**Misconception**: "The data I'm rendering is safe because it comes from my database"
**Socratic opener**: "I see `dangerouslySetInnerHTML` at `[file:line]`. The name has 'dangerously' in it — what do you think it's warning about?"

### Missing Authentication on API Routes

**Grep**: API route handlers without auth checks
```bash
grep -rn "export.*async.*GET\|export.*async.*POST\|export.*async.*PUT\|export.*async.*DELETE" src/app/api/ 2>/dev/null
```
Then check if those files contain auth/session checks.
**Risk**: Anyone with the URL can access the endpoint — no login required
**Misconception**: "Nobody knows my API URLs" or "only my frontend calls these"
**Socratic opener**: "Your API route at `[file:line]` handles requests. What stops someone who isn't logged in from calling it directly?"

---

## Priority 2: Data Integrity

### Unhandled Async Errors

**Grep**: `fetch()` or `await` without try/catch
```bash
grep -rn "await.*fetch\|await.*axios" src/ 2>/dev/null
```
Then check if those lines are inside try/catch blocks.
**Risk**: Network failure crashes the page or silently fails
**Misconception**: "APIs always respond" or "await handles errors automatically"
**Socratic opener**: "This fetch call at `[file:line]` talks to an external service. What happens to your app if that service is down?"

### Missing Null/Undefined Checks

**Grep**: Property access chains without optional chaining
```bash
grep -rn "\w\+\.\w\+\.\w\+" src/ --include="*.ts" --include="*.tsx" 2>/dev/null | grep -v "?." | head -10
```
**Risk**: TypeError crashes when data is unexpectedly missing
**Misconception**: "The data is always there because it worked in testing"
**Socratic opener**: "This line at `[file:line]` accesses `[a.b.c]`. What happens if `[b]` is null?"

### No Database Transaction / Atomic Operations

**Grep**: Multiple sequential database writes without transactions
```bash
grep -rn "\.create\|\.update\|\.delete\|\.insert\|\.save" src/ 2>/dev/null | head -10
```
**Risk**: Partial writes leave data in inconsistent state
**Misconception**: "Each line runs successfully if the one before it did"
**Socratic opener**: "I see two database writes in a row at `[file:line]`. What happens if the first one succeeds but the second one fails?"

---

## Priority 3: Reliability

### No Loading / Error States in UI

**Grep**: `useState` for data without corresponding loading/error states
```bash
grep -rn "useState.*null\|useState.*\[\]\|useState.*undefined" src/ 2>/dev/null | head -10
```
**Risk**: Users see blank screens, broken layouts, or stale data
**Misconception**: "The data loads instantly" or "errors don't happen in production"
**Socratic opener**: "This component at `[file:line]` starts with empty data and fetches it. What does the user see in that moment before the data arrives?"

### Polling Without Cleanup

**Grep**: `setInterval` without corresponding `clearInterval`
```bash
grep -rn "setInterval" src/ 2>/dev/null
```
Then check if the same scope has `clearInterval`.
**Risk**: Memory leaks, multiple timers stacking, excessive API calls
**Misconception**: "The timer stops when I leave the page"
**Socratic opener**: "You have a `setInterval` at `[file:line]` that checks for updates every few seconds. What happens when the user navigates away from this page?"

### No Rate Limiting on API Routes

**Grep**: API routes without rate limiting
```bash
ls src/app/api/ 2>/dev/null
```
Check for rate limiting middleware or library imports.
**Risk**: Bots can hammer endpoints, rack up API costs, cause outages
**Misconception**: "Only real users will call my API"
**Socratic opener**: "Your API has [N] public endpoints. What stops someone from writing a script that calls them 10,000 times per minute?"

---

## Priority 4: Maintainability

### Magic Numbers / Hardcoded Values

**Grep**: Numbers used directly in logic without named constants
```bash
grep -rn "setTimeout.*[0-9]\{4,\}\|setInterval.*[0-9]\{4,\}" src/ 2>/dev/null
grep -rn "if.*===.*[0-9]\{2,\}" src/ 2>/dev/null | head -5
```
**Risk**: Nobody (including future you) will know what 86400000 means
**Misconception**: "I'll remember what this number means"
**Socratic opener**: "I see the number `[value]` at `[file:line]`. Without looking at the context — do you remember what it means?"

### Giant Components (200+ lines)

**Grep**: Check file sizes of components
```bash
wc -l src/**/*.tsx 2>/dev/null | sort -rn | head -10
```
**Risk**: Hard to understand, hard to test, hard to change without breaking things
**Misconception**: "It works, so the structure doesn't matter"
**Socratic opener**: "Your component at `[file]` is [N] lines long. If you needed to change just the [X part], how confident are you that you wouldn't accidentally break [Y part]?"

### No TypeScript Types on API Boundaries

**Grep**: API responses used without type definitions
```bash
grep -rn "response\.json()\|\.json()" src/ 2>/dev/null | head -10
```
**Risk**: Shape of data can change without warning, causing runtime crashes
**Misconception**: "The API always returns the same thing" or "TypeScript catches everything"
**Socratic opener**: "You're using data from an API response at `[file:line]` without defining its shape. What happens if the API changes what it sends back?"

---

## How to Use in Teaching

### During `/learn audit` mode
Work through findings in priority order (security first). For each:
1. Show the code location
2. Ask the Socratic opener (Level 1)
3. Follow the hint ladder if needed
4. Connect to the underlying concept
5. Update progress file

### During other modes (opportunistic)
If you spot a risk pattern in the files being discussed:
- Don't derail the current lesson
- Note it: "I also noticed something in this file that's worth understanding later"
- If the current concept is related (e.g., teaching async and you spot unhandled errors), weave it in naturally

### Never use scare tactics
Don't say "this is a security vulnerability" or "this is dangerous." Instead:
- "What could happen here that you didn't plan for?"
- "Who else, besides your intended users, could interact with this?"
- "What's the worst case if this fails?"

Let them discover the risk through questions. The realization is more durable than the warning.
