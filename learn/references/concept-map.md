# Extended Concept Map

Maps code patterns to teachable concepts. Each concept includes multi-background analogies, mental model prerequisites, and what it unlocks.

## How to Use This File

1. **During concept extraction**: Match diff patterns to concepts using the tables below
2. **During prerequisite checking**: Use the dependency graph to verify readiness
3. **During teaching**: Pick analogies that match the learner's `analogy_domains` from their profile
4. **During `/learn next`**: Use the unlock graph to find the highest-value next concept

## Tier 1: Foundations

| Pattern | Concept | Analogies by Background |
|---|---|---|
| `<div>`, `<span>`, JSX | HTML elements | **Design:** Frames in Figma. **PM:** Sections on a brief. **Data:** Cells in a spreadsheet. **General:** Lego bricks. |
| `props` passed to components | Component communication | **Design:** Layer properties in Figma. **PM:** Form fields. **Data:** Function parameters. **General:** Settings you pass to a machine. |
| `children` prop | Component composition | **Design:** A frame that holds other frames. **PM:** A template with a content slot. **General:** A picture frame — the component is the frame, children are the photo. |
| `key` prop in `.map()` | List rendering identity | **Design:** Layer names. **PM:** Ticket IDs. **Data:** Primary keys. **General:** Name badges — React needs to track who's who. |
| `src/app/page.tsx` | File-based routing | **Design:** Artboard names become URLs. **PM:** Sitemap. **Data:** Folder = table. **General:** Filing cabinet — folder path maps to web address. |
| `'use client'` | Server vs client components | **Design:** Exported assets vs interactive prototypes. **PM:** Reports vs dashboards. **General:** Kitchen vs dining room. |
| `import` / `export` | Module system | **Design:** Component libraries. **Data:** Library imports. **General:** Published papers — you cite (import) others' work. |
| `index.ts` barrel files | Public API surface | **Design:** A component's public props vs internal state. **PM:** Public roadmap vs internal backlog. **General:** Reception desk. |

**Mental model prerequisites for Tier 1:**
- "A website is made of individual files that the browser reads"
- "Components are reusable pieces — like templates"
- "The URL in the browser maps to a specific file in the project"

## Tier 2: Interactivity

| Pattern | Concept | Analogies by Background |
|---|---|---|
| `useState(initialValue)` | Component state | **Design:** Toggle state in a prototype. **PM:** Status field on a ticket. **Data:** A variable that triggers recalculation. **General:** A whiteboard the component uses as a scratchpad. |
| `useEffect(() => {}, [deps])` | Side effects & lifecycle | **Design:** Triggering animation on layer visibility. **PM:** Recurring check — run on load, clean up on close. **Data:** A triggered procedure. **General:** Setup and teardown of equipment. |
| `useEffect` dependency array | Reactive dependencies | **Design:** Auto-layout constraints. **PM:** Notification rules. **Data:** Watch list in a query. **General:** A filter — only react to specific changes. |
| `useCallback`, `useMemo` | Performance optimization | **Data:** Caching query results. **PM:** Saving a report template. **General:** Don't redo work that hasn't changed. |
| `useContext` | Shared state | **Design:** Global styles vs per-component overrides. **PM:** Company settings vs team settings. **General:** Office-wide announcement vs passing notes. |
| Tailwind classes | Utility-first CSS | **Design:** Quick inline styles vs a design system. **PM:** Ad-hoc formatting vs templates. **General:** Sticky notes vs a style guide. |
| `var(--token)` | CSS custom properties | **Design:** Design tokens. **PM:** Spreadsheet variable. **Data:** Named constant. **General:** Change once, update everywhere. |
| `npm run build` | Build process | **Data:** Compiling a notebook into a report. **PM:** Draft → published doc. **General:** Raw ingredients → finished product. |
| `.env.local` | Environment config | **PM:** Different settings for staging vs prod. **General:** Different contact lists per office. |

**Mental model prerequisites for Tier 2:**
- "Components re-render when their data changes" (requires: component state)
- "Some code runs at specific moments — not just top-to-bottom" (requires: lifecycle)
- "The browser and server are different environments" (requires: server vs client)

## Tier 3: Data Flow

| Pattern | Concept | Analogies by Background |
|---|---|---|
| `fetch(url, { method })` | HTTP methods | **PM:** JIRA actions — create/view/edit/delete. **Data:** CRUD operations. **General:** POST = submit, GET = check, PUT = update, DELETE = remove. |
| `response.json()` | Parsing responses | **Data:** CSV → dataframe. **General:** Translating a reply into a usable format. |
| `try { await fetch() } catch` | Network error handling | **PM:** Fallback plan when a vendor doesn't deliver. **General:** Plan B when something fails. |
| `res.status(400)` | HTTP status codes | **PM:** Approval workflow — 200=approved, 400=fix your form, 500=our system is down. **General:** Traffic lights for requests. |
| Headers | HTTP metadata | **General:** The envelope around a letter — routing info, not content. |
| `setInterval(poll, 5000)` | Polling | **General:** Checking email every 5 min vs getting push notifications. |
| Webhook handler | Webhooks | **PM:** "Notify me when this ticket moves" vs checking the board hourly. **General:** "Call me when ready" vs "Are you done yet?" |
| Rate limiting, retry | API reliability | **General:** Don't spam — wait, then try again politely. |
| `JSON.stringify/parse` | Serialization | **Data:** Converting between file formats. **General:** Packing and unpacking. |
| `?key=value` | Query parameters | **General:** Search form inputs become part of the URL. |
| CORS errors | Cross-origin security | **General:** Badge access — browser checks if the API allows requests from your domain. |

**Mental model prerequisites for Tier 3:**
- "Your app talks to other computers over the internet" (requires: client/server)
- "Requests can fail — the network is unreliable" (requires: async)
- "Data has a shape that both sides need to agree on" (requires: types)

## Tier 4: Architecture

| Pattern | Concept | Analogies by Background |
|---|---|---|
| State machine transitions | Finite state machines | **PM:** Ticket workflow: backlog → in progress → review → done. **Design:** Prototype states. **General:** Steps with valid transitions — no skipping. |
| `Promise.all([...])` | Parallel async | **PM:** Multiple workstreams. **Data:** Parallel queries. **General:** Cooking multiple dishes at once. |
| `AbortController` | Cancellation | **General:** Stopping a recording mid-call — clean shutdown, not pulling the plug. |
| Database CRUD | Persistent data | **Data:** SQL operations. **PM:** Spreadsheet rows. **General:** Filing cabinet: add, read, edit, remove. |
| `uuid()` | Unique identifiers | **PM:** Ticket IDs. **Data:** Primary keys. **General:** Globally unique, no collisions. |
| Middleware | Request interceptors | **General:** Security checkpoint — every request passes through. |
| `z.object({...})` | Runtime validation | **PM:** Form validation. **Data:** Schema validation. **General:** Check inputs before processing. |
| `type` vs `interface` | TypeScript structural types | **PM:** Job description vs candidate profile. **Data:** Schema vs data type. **General:** Both define shape. |
| `<T>` generics | Parameterized types | **PM:** Template for any project type. **Data:** Function for any column. **General:** A form that works for any category. |
| `null` vs `undefined` | Absence semantics | **Data:** NULL in SQL vs missing column. **General:** "No answer" (undefined) vs "Answered: none" (null). |
| Error boundaries | Graceful UI failure | **General:** One broken room doesn't close the building. |

**Mental model prerequisites for Tier 4:**
- "Programs have states and transitions between them" (requires: conditionals, state)
- "Multiple things can be 'in flight' at the same time" (requires: async/await)
- "Data needs to survive page refreshes and server restarts" (requires: client/server)

## Tier 5: Security & Production

These are especially important for vibe coders who accept AI-generated code without understanding the risks. See `references/vibe-coder-risks.md` for the full risk catalog with grep patterns.

| Pattern | Concept | Vibe Coder Risk |
|---|---|---|
| No input validation on API routes | Input validation | Accepts any data, including malicious |
| Secrets in client-side code | Secret management | API keys visible in browser DevTools |
| `dangerouslySetInnerHTML`, `eval()` | XSS prevention | Users can inject executable code |
| No auth checks on API routes | Authorization | Anyone with the URL can access the endpoint |
| No error boundaries | Graceful degradation | One error crashes the whole app |
| No rate limiting | API abuse prevention | Bots can hammer endpoints, rack up costs |
| Hardcoded credentials | Credential hygiene | Secrets committed to git history forever |
| No HTTPS / mixed content | Transport security | Data readable by anyone on the network |

**Mental model prerequisites for Tier 5:**
- "Your API is public — anyone can call it, not just your frontend" (requires: API routes)
- "The browser can't keep secrets — everything in client code is visible" (requires: server vs client)
- "Users are not always well-intentioned" (requires: input handling)

## Concept Dependency Graph

Check that prerequisites are understood (confidence >= 3) before teaching the concept.

### Code Prerequisites
```
async/await → Promises, callbacks
useEffect → useState, component lifecycle
API routes → HTTP methods, JSON
TypeScript types → JavaScript objects
Error handling → try/catch, async/await
State machines → TypeScript types, status enums
Webhooks → API routes, HTTP methods
Design tokens → CSS basics, variables
Module boundaries → import/export, file-based routing
Testing → functions, assertions, expected vs actual
Input validation → TypeScript types, API routes
XSS prevention → HTML rendering, user input
Authorization → API routes, session/token concepts
```

### Mental Model Prerequisites
```
async/await:
  - "JavaScript runs one thing at a time (single-threaded)"
  - "A function can return before its work is done"
  - "Waiting doesn't mean the whole program stops"

useEffect:
  - "Components are functions that run again when data changes"
  - "Some work needs to happen at specific moments, not just on render"

API routes:
  - "The browser and server are separate machines"
  - "They communicate by sending structured messages back and forth"

Error handling:
  - "Things fail — networks, APIs, user input, disk space"
  - "Failing gracefully is better than crashing"

Security:
  - "Your app is accessible to everyone on the internet"
  - "Not everyone who accesses your app has good intentions"
  - "The browser shows all your client-side code to anyone who looks"
```

### Unlock Graph (what learning this enables)
```
async/await → unlocks: API calls with error handling, database queries, file operations, Promise.all
API routes → unlocks: webhooks, authentication, input validation, rate limiting
TypeScript types → unlocks: generics, Zod validation, API contracts, type guards
useState → unlocks: useEffect, useContext, state machines, form handling
Error handling → unlocks: graceful degradation, retry logic, error boundaries
Module system → unlocks: barrel exports, lazy loading, code splitting
HTTP methods → unlocks: REST API design, CRUD operations, webhooks
```

## Common Misconceptions

When detected (through wrong answers or incorrect explanations), these point to specific teaching needs.

| Misconception | What they said | What to teach |
|---|---|---|
| "await blocks the whole program" | "Everything stops while it waits" | Event loop, single-threaded concurrency |
| "async makes things parallel" | "It runs on multiple cores" | Concurrency vs parallelism |
| "the browser runs API code" | "My API route runs in the browser" | Client/server separation |
| "state updates instantly" | "I set it and read it on the next line" | React's batched async state updates |
| "types prevent all bugs" | "TypeScript catches everything" | Type safety scope, runtime vs compile-time |
| "env vars are always hidden" | "Nobody can see my keys" | Client-side env var exposure |
| "the API only gets requests from my app" | "Only my frontend calls it" | APIs are public endpoints |
| "it worked in testing so it's fine" | "Tests passed, ship it" | Edge cases, error paths, production differences |
