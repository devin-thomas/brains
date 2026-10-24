# Techniques

Recurring techniques in Matt Pocock's work, in two groups: working with AI coding agents and TypeScript practice. Each entry is what the sources say, cited, or a synthesis across several cited passages. Detail is in `knowledge/`.

Skills named here are from his MIT-licensed skills repository, a snapshot from October 2026. He says he updates the skills regularly. [i-stopped-using-grill-me-for-coding @ 00:14:22]

## Working with coding agents

### The main flow

The router skill encodes the flow. Start with `/grill-with-docs`; detour through `/prototype` if a question needs runnable code; for multi-session work use `/to-spec` then `/to-tickets`; build with `/implement` per ticket or `/implement-spec` for the whole spec; finish with `/code-review`, and close the loop with `/retro`. [ask-matt § The main flow: idea → ship] It advises keeping the grilling, spec and tickets in one unbroken context window, and each `/implement` fresh. [ask-matt § The main flow: idea → ship › Context hygiene]

He splits work into human-in-the-loop tasks (planning and alignment) and AFK tasks (implementation), calling the first the day shift and the second the night shift. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:26:00] [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:46:00]

### Grill, then plan

The grilling skill maps the conversation as a design tree and asks the whole frontier of ready questions in a round, each with a recommended answer worded so that "yes" accepts it. [grilling § Source text] Finding facts is the agent's job, so it dispatches a sub-agent to look them up, and the decisions stay with the user. [grilling § Source text] It ends when the frontier is empty and the user confirms. [grilling § Source text]

His theory for why it works is that hiding the next step from the agent keeps it in the interview stage rather than rushing to a plan. [agent-skills-with-matt-pocock @ 00:05:00] He separates questions answerable in conversation from high-fidelity questions about feel, which should go to a prototype through a handoff. [nine-things-wrong-with-grill-skills @ 00:00:00]

Choosing the amount of planning: for a small diff, use a one-shot prompt; for larger work, grill; if the interview exposes something too large for one planning session, use Wayfinder. [when-should-you-not-use-grill-me § Detailed source notes]

### Grow a glossary during the interview

The domain-modeling skill challenges the user's terms against the glossary, proposes a canonical term when language is vague or overloaded, stress-tests relationships with invented edge-case scenarios and cross-references the code. [domain-modeling § During the session › Challenge against the glossary] [domain-modeling § During the session › Cross-reference with code] It updates the glossary inline as terms resolve, and the glossary must be devoid of implementation details. [domain-modeling § During the session]

It offers an ADR only when the decision is hard to reverse, surprising without context, and the result of a real trade-off. [domain-modeling § During the session › Offer ADRs sparingly] A single-context repository has one glossary at the root; a multi-context one has a map file pointing at a glossary and an `adr` folder per context. [domain-modeling § File structure]

### Spec as a destination, tickets as vertical slices

The `/to-spec` skill does not interview: it synthesizes what the conversation and codebase already show. [to-spec § Source text] It sketches test seams first and prefers the highest seam, with one seam ideal, and says not to include file paths or code snippets, which go stale. [to-spec § Process] [to-spec § Implementation Decisions] He treats a spec as a short-lived artifact that schedules work across sessions, and does not favour persisting specs. [agent-skills-with-matt-pocock @ 00:17:30]

`/to-tickets` breaks the work into tracer-bullet vertical slices, each cutting a narrow but complete path through every layer and sized to fit in a single fresh context window. [to-tickets § Process › 3. Draft vertical slices] Each ticket declares blocking edges, so a ticket whose blockers are done is on the frontier. [to-tickets § Process › 3. Draft vertical slices] Before publishing, the skill quizzes the user on granularity and blocking edges until they approve. [to-tickets § Process › 4. Quiz the user] He asks for vertical slices because AI tends to code horizontally, layer by layer, which delays feedback. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:40:00]

### Wayfinder for foggy work

Wayfinder is for a large idea with unresolved questions that cannot fit one agent session. [new-skills-v1-1-wayfinder-research-implement @ 00:07:53] It charts a shared map on the issue tracker, then works decision tickets one at a time, producing decisions, not deliverables. [wayfinder § Source text] The map is an index, not a store. [wayfinder § The Map] Tickets are typed research, prototype, grilling or task. [wayfinder § Ticket Types] Out-of-view decisions sit as "fog of war" in a Not yet specified section. [wayfinder § Fog of war] When the map clears it hands off at `/to-spec` rather than building. [ask-matt § On-ramps]

### Prototype to answer a question

When a design or logic question needs a runnable answer, he advises prototyping instead of writing a long spec. [dont-waste-time-on-specs-prototype-instead @ 00:00:00] The skill defines a prototype as throwaway code that answers a question, either a single HTML logic demo or several UI variants on one route. [prototype § Source text] [prototype § Pick a branch] The prototype is kept as a primary source on a throwaway branch, with a pointer from the implementation issue, while main keeps only the validated decision. [prototype § Rules that apply to both]

### Test first, at agreed seams

Watching a test fail and then pass makes the agent's work legible, and he warns against a big batch of tests followed by a one-shot implementation. [red-green-refactor-is-op-with-claude-code @ 00:02:00] [red-green-refactor-is-op-with-claude-code @ 00:03:01] The tdd skill states red before green, one slice at a time, test only at pre-agreed seams, and verify behaviour through public interfaces. [tdd § Rules of the loop] [tdd § Seams: where tests go] [tdd § What a good test is]

It requires writing down the seams under test and confirming them with the user before any test is written. [tdd § Seams: where tests go] Its anti-patterns are implementation-coupled tests, tautological tests and horizontal slicing. [tdd § Anti-patterns] The tell for a coupled test is that it breaks on a refactor when behaviour has not changed. [tdd § Anti-patterns]

For bugs, a regression test is written before the fix only if there is a correct seam, one where the test exercises the real bug pattern as it occurs at the call site; if no correct seam exists, that is itself the finding. [diagnosing-bugs § Phase 5: Fix + regression test]

### Check the checks: mutation testing

A tool edits the source and reruns the suite, and surviving mutants give the agent rich feedback to write stronger tests. [mutation-testing-is-awesome @ 00:00:16] [mutation-testing-is-awesome @ 00:00:31] Robert C. Martin, in conversation with Matt, describes a "verification gauntlet" of coverage, static checks and mutation testing; that is Martin's practice, not Matt's. [live-uncle-bob-software-fundamentals-ai @ 00:00:00]

### Review on two axes, in separate sub-agents

The code-review skill checks the diff against a fixed point on two axes, Standards and Spec, run as parallel sub-agents so they do not pollute each other's context. [code-review § Source text] Standards always carries a baseline of Fowler code smells, and the repository's documented standards override it. [code-review § Process › 3. Identify the standards sources] The skill does not merge or rerank the axes, because a change can pass one axis and fail the other. [code-review § Process › 5. Aggregate] [code-review § Why two axes]

A `/pr` skill classifies changes as one-way or two-way doors and adds merge-danger and blast-radius summaries. [fixing-the-pr-bottleneck-matt-pocock-aihero @ 00:16:00]

### Retro: change the environment, not the code

`/retro` looks back over a session and suggests changes to the agent's environment. [ask-matt § The main flow: idea → ship] Its categories are navigation, automated checks, coding standards, bloated global instructions, tool economy, no-ops and information access. [retro § Steps] Mechanical mistakes get a deterministic check, `CODING_STANDARDS.md` is reserved for judgement calls, and the suggestions go to a human for approval. [retro § Steps] [new-skills-v1-3-pr-implement-spec-retro @ 00:10:30]

### Handoff instead of compaction

At a phase boundary the router orders the choices as continue, clear, hand off, subagent or compact, and reserves handoff for a new harness, a new directory, a colleague, or forking a side task. [ask-matt § Phase boundaries] He dislikes compacting. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:07:30] The handoff skill writes a summary to the OS temp directory, points to other artifacts instead of copying them, names suggested skills, and redacts keys and personal data. [handoff § Source text]

### Unattended implementation

His Ralph loop script is bounded by a maximum iteration count and a completion marker, and feeds each run a JSON requirements document and a progress log. [ship-working-code-while-you-sleep-with-the-ralph-wiggum-technique @ 00:04:50] By June 2026 he said a loop is only one pattern and preferred an away-from-keyboard agent that takes one scoped task from a queue. [matt-pocock-s-agentic-engineering-workflow-just-copy-him @ 00:42:00]

Sandcastle is his TypeScript library that runs a prompt, an agent and a sandbox together, because unattended agents with permission checks off need isolation. [open-sourced-afk-software-factory @ 00:00:00] `/implement-spec` treats tickets as a task graph and runs implementer subagents across the ready frontier, each in its own worktree and driving `/tdd`, with a merger subagent and one review at the end. [implement-spec § Source text] [implement-spec § Steps]

### Hallucination and injection triage

His diagnosis for a coding hallucination starts with one question: was the information in the context window? If not, it is a factuality hallucination and the fix is to load the information. [my-agent-hallucinated-what-do-i-do @ 00:00:03] [my-agent-hallucinated-what-do-i-do @ 00:00:16] If it was there and the agent ignored it, it is a faithfulness hallucination, partly attention degradation, and the remedy is to leave the dumb zone. [my-agent-hallucinated-what-do-i-do @ 00:00:43] [my-agent-hallucinated-what-do-i-do @ 00:01:20]

For prompt injection, he recommends putting retrieved data in user messages inside XML tags that mark both start and end, because moving the data into user messages alone did not fix his demo. [the-hidden-security-risk-in-your-system-prompts @ 00:01:22] [the-hidden-security-risk-in-your-system-prompts @ 00:01:51]

### Deepening a codebase

The `codebase-design` skill gives the shared vocabulary: module, interface, depth, seam, adapter, leverage and locality. [codebase-design § Glossary] Its deletion test imagines deleting a module: if the complexity vanishes it was a pass-through, and if it reappears across many callers the module was earning its keep. [codebase-design § Principles] One adapter means a hypothetical seam and two adapters a real one. [codebase-design § Principles]

`/improve-codebase-architecture` weights recently changed parts of the codebase, reads the glossary and ADRs, has a sub-agent walk the code noting friction, and writes a self-contained HTML report of candidates. [improve-codebase-architecture § Process › 1. Explore] [improve-codebase-architecture § Process › 2. Present candidates as an HTML report] The chosen candidate is then grilled. [improve-codebase-architecture § Process › 3. Grilling loop] It is not an away-from-keyboard skill: the agent is the sergeant who finds candidates and the human is the general who decides. [how-to-de-slop-a-codebase-ruined-by-ai § Detailed source notes]

### Writing for agents

The writing guide's levers: content sits on an information hierarchy of in-file steps, in-file reference and disclosed reference behind pointers. [writing-for-agents § Information hierarchy] Every step ends on a clear completion criterion. [writing-for-agents § Steps and completion criteria] Leading words are compact concepts already in the model's pretraining, such as "tracer bullets", repeated as a token, and negation drags the forbidden behaviour into context so the positive should be prompted. [writing-for-agents § Leading words] For pruning it asks for a single source of truth and hunts sediment and no-ops. [writing-for-agents § Pruning]

One cheap output instruction is to tell the model to be extremely concise and sacrifice grammar, which he keeps in his global CLAUDE.md. [tell-your-llm-to-sacrifice-grammar @ 00:00:00] [make-claude-code-give-you-answers-not-essays @ 00:00:17]

## TypeScript practice

### Choose the tsconfig by one fork

The first real decision is whether TypeScript itself emits the JavaScript. [the-tsconfig-cheat-sheet @ 00:02:10] If it does, he gives `NodeNext` for module and resolution, an `outDir` of `dist` and source maps. [the-tsconfig-cheat-sheet @ 00:02:10] If something else runs the code, use `bundler` resolution with `module` `ESNext` and `noEmit` true. [the-tsconfig-cheat-sheet @ 00:02:50] On every project he sets `strict`, adds `noUncheckedIndexedAccess`, sets `skipLibCheck` and `esModuleInterop`, and leaves out `noUnusedLocals`. [the-tsconfig-cheat-sheet @ 00:00:35] [the-tsconfig-cheat-sheet @ 00:01:30]

### Generic or not

The first question is whether the function must handle unknown types: if every possible input and its type are known before the call, ordinary branching is enough. [when-should-i-use-a-generic @ 00:00:00] Use a generic when the return type must track the input, because a union return type would not preserve the link between a particular input and its result. [when-should-i-use-a-generic @ 00:00:50] When inference becomes hard to express, he advises simplifying the function or the runtime API. [advanced-typescript-lets-learn-generics @ 01:28:15]

### Derive from the value

A route object written plainly has `string` values and fails when passed to a function that accepts only the specific route strings. [as-const-the-most-underrated-typescript-feature @ 00:01:00] `as const` narrows each value to its literal type, `typeof` recovers the type, `keyof` gives the keys, and indexing with that union gives the values, so a new entry flows through without further edits. [as-const-the-most-underrated-typescript-feature @ 00:02:00] [as-const-the-most-underrated-typescript-feature @ 00:03:20]

### satisfies or annotation

A variable annotation operates on the variable and widens its type, while `satisfies` only checks the value. [most-ts-devs-dont-understand-satisfies @ 00:00:32] Use an annotation when you want the wider type, and `satisfies` when you want narrow, specific keys and autocomplete from a checked config. [most-ts-devs-dont-understand-satisfies @ 00:01:20] [most-ts-devs-dont-understand-satisfies @ 00:02:00]

### Discriminated unions for state

An interface with `status`, `error` and `data` properties allows states that should not exist, and the fix is a union of object types each carrying a `status` discriminant. [you-have-to-know-this-typescript-pattern @ 00:00:02] [you-have-to-know-this-typescript-pattern @ 00:00:49] His signal for needing one is a state object with several optional properties. [you-have-to-know-this-typescript-pattern @ 00:01:24]

### Narrow early

Throwing early when a required element is missing is his preferred response, because after the guard TypeScript narrows `HTMLElement | null` to `HTMLElement`. [typescript-loves-errors @ 00:00:16] Narrowing an `unknown` needs only control-flow checks. [turn-unknown-into-anything @ 00:00:19] With `noUncheckedIndexedAccess` the indexed value becomes possibly `undefined` and narrows after a check. [no-unchecked-indexed-access @ 00:00:44]

### Branded types and value objects

A branded type adds a compile-time tag to a string so two values that are both strings at runtime can be told apart. [branded-types-have-saved-my-ass-so-many-times @ 00:00:00] His motivating bug is a monorepo where `readdir` returns relative paths that code expecting absolute paths can use to delete or create the wrong directory. [branded-types-have-saved-my-ass-so-many-times @ 00:01:00] The cost is that values must be converted with `as` assertions. [branded-types-have-saved-my-ass-so-many-times @ 00:01:24]

### Reach into types you do not control

`Parameters` extracts a parameter type from a function inside `node_modules`. [parameters-is-so-helpful @ 00:00:36] For an unexported SDK type he wraps a union in `NonNullable` and uses `Extract` on the `type` field to isolate one member. [library-types-can-run-but-they-cant-hide @ 00:00:33] [library-types-can-run-but-they-cant-hide @ 00:00:53]

### Type the environment and validate at the edge

His video makes the global `ProcessEnv` interface extend the inferred type of a Zod env schema, so keys are declared once and autocomplete on `process.env`; he names the limit that values are always strings. [youre-typing-process-env-wrong @ 00:40] [youre-typing-process-env-wrong @ 02:03] The 2024 article adds runtime validation with t3-env, which checks values at the boundary. [how-to-strongly-type-process-env § Research note]

### Migrate by order and size

Migrations fail for two reasons, scope (one massive pull request) and order (converting a file while the files it imports are still JavaScript forces loose escape hatches). [why-typescript-migrations-fail § Detailed source notes] In 2024 he said to set the strictness level first, then convert one file at a time, starting with files that have no dependencies and that others depend on. [dejavue-e023-typescript-and-content-creation-with-matt-pocock @ 21:00]

### Type React with the narrow vocabulary

`React.ReactNode` covers every renderable value and is the type he needs about 99 percent of the time for slots that accept content. [reacts-most-useful-types-matt-pocock-react-day-berlin-2023 @ 04:15] `React.ComponentProps` is for third-party components whose props are not exported, and `React.ElementRef<typeof Component>` gives the ref type for components you do not control. [reacts-most-useful-types-matt-pocock-react-day-berlin-2023 @ 12:16] [reacts-most-useful-types-matt-pocock-react-day-berlin-2023 @ 18:58]

### Test types

His article treats type behaviour as part of a library's public contract and compares Vitest, small custom assertion helpers and the `tsd` package. [how-to-test-your-types § Research note] For tests that need a complex typed object, `fromPartial` accepts a subset of the target type. [testing-in-typescript-sucks-lets-make-it-better @ 00:09:00]

## Teaching technique

He opens with the failing code and then the fix. [string-literals-are-great @ 00:00:11] He uses images of sharpness, a cast as the sharpest knife and a scalpel against a sledgehammer. [dynamic-objects-should-not-be-this-hard @ 00:02:50] [the-typescript-hill-im-willing-to-die-on @ 00:01:23] He gives a verdict with a decision rule rather than a slogan: enums are avoided in new code and flagged in review, without rejecting existing pull requests. [enums-considered-harmful § Detailed source notes]

For learning with an agent, his `/teach` skill is stateful: it keeps a mission, a learning record, a glossary and lessons so each session adapts to the learner. [learn-anything-with-the-teach-skill @ 00:00:00] [learn-anything-with-the-teach-skill @ 00:02:40]

## Applying this

- *Inference:* for a feature larger than one session, run the main flow: grill with docs, spec, vertical-slice tickets, implement each in a fresh context, review in a separate context, retro.
- *Inference:* for a one-session change, run a grilling round and implement directly.
- *Inference:* when an agent fails, ask first whether the fact was in context, then whether the context was too full, before changing the model or the prompt.
- *Inference:* when asked a TypeScript question, ask whether the code is application, library or shared utility, and whether the input is trusted, before giving a rule.
