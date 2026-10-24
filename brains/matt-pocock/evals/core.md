# Evals: core

Run each question against an agent given only this brain ([AGENTS.md](../AGENTS.md) as its instructions). A pass needs the expected facts, a citation that resolves to the listed heading or timestamp, a date on any position that changed, and no unsupported attribution to Matt Pocock. Figures are his reported figures at recording time.

## Fact recall (TypeScript)

| # | Question | Must include | Evidence |
| --- | --- | --- | --- |
| 1 | What does `satisfies` do that a variable annotation does not? | Checks a value against a type without widening it, so narrow keys and autocomplete survive; an annotation widens the variable | [most-ts-devs-dont-understand-satisfies @ 00:00:32] [most-ts-devs-dont-understand-satisfies @ 00:02:50] |
| 2 | Which `tsconfig` strictness options does his cheat sheet recommend, and which does he call noise? | `strict: true` (libraries assume it) and `noUncheckedIndexedAccess`; `noUnusedLocals` and `noUnusedParameters` are noise | [the-tsconfig-cheat-sheet @ 00:01:30] |
| 3 | What is the empty object type `{}` in TypeScript? | Accepts every value except `null` and `undefined`; not "an object with no properties" | [the-empty-object-is-typescripts-weirdest-type @ 00:00:01] |
| 4 | Why does he recommend property syntax over method shorthand in object types? | Method shorthand can permit unsafe parameter substitutions that property syntax rejects under strict function checking | [method-shorthand-syntax-considered-harmful § Research note] |
| 5 | What are the three operations he groups under "generics"? | Passing types to other types, passing types to functions, inferring type arguments from values | [typescript-generics-in-three-easy-patterns § Research note] |
| 6 | What does `NoInfer` do? | Blocks a parameter position from contributing inference candidates, so a state machine's `initial` is checked against declared states instead of widening them | [8-typescript-tips-to-expand-your-mind-and-improve-your-code @ 00:06:12] [noinfer § Research note] |

## His recommendations

| # | Question | Must include | Evidence |
| --- | --- | --- | --- |
| 7 | Where does he say Zod belongs? | At untrusted edges (user input, webhooks, API responses); skip duplicate checks on values controlled inside the application | [when-should-you-use-zod § Research note] |
| 8 | Which `any`-versus-`unknown` position does he take for a shared group-by helper? | `any` causes bugs, `unknown` forces manual narrowing, neither is the answer; generics are | [solving-the-any-problem-is-harder-than-you-think @ 00:03:52] [solving-the-any-problem-is-harder-than-you-think § Detailed source notes] |
| 9 | When does he say to use `NodeNext` versus `bundler` module resolution? | `NodeNext` when `tsc` emits for Node (imports need `.js` extensions); `bundler` when something else runs or bundles the code | [the-tsconfig-cheat-sheet @ 00:02:30] [the-tsconfig-cheat-sheet @ 00:02:50] [js-files-in-typescript-why @ 00:00:31] |

## Changed over time

| # | Question | Pass if the answer | Evidence |
| --- | --- | --- | --- |
| 10 | Should I use `type` or `interface`? Has his advice changed? | Dates both: 2022 `type` default (video later labelled deprecated); 2025 correction that the TS wiki contrasted interfaces with intersections, so prefer `interface extends` over intersections and treat plain-object type vs interface as immaterial | [typescript-types-or-interfaces-deprecated-2022 § Detailed source notes] [types-vs-interfaces-what-i-got-wrong-in-2022 @ 00:00:44] [types-vs-interfaces-what-i-got-wrong-in-2022 @ 00:09:16] |
| 11 | How big is the smart zone, per Matt? | Gives the series with dates (about 100K tokens April 2026, about 120K May, about 150K July), says the framing is credited to Dex Horthy, and prefers the latest | [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:03:00] [nine-things-wrong-with-grill-skills @ 00:03:43] [what-is-the-dumb-zone @ 00:00:53] |
| 12 | How has `/grill-me` changed? | Version 1.1 (July 2026) one question at a time; then rounds of dependent questions; the current skill text covers the whole ready frontier per round; plain `/grill-me` replaced by `/grill-with-docs` for codebase work from May 2026 | [new-skills-v1-1-wayfinder-research-implement @ 00:00:37] [this-change-makes-grill-me-so-much-better @ 00:01:01] [grilling § Source text] [i-stopped-using-grill-me-for-coding @ 00:00:00] |

## AI-engineering practice

| # | Question | Must include | Evidence |
| --- | --- | --- | --- |
| 13 | What is the flow in his skills repo from idea to shipped? | `/grill-with-docs`, optional `/prototype`, `/to-spec` then `/to-tickets`, `/implement` per ticket or `/implement-spec`, `/code-review`, `/retro` | [ask-matt § The main flow: idea → ship] |
| 14 | What does he say to do at the edge of the context window? | Clear, compact, hand off or redesign the task; he dislikes compacting; handoff is reserved for a new harness, directory, colleague or side task | [what-is-the-dumb-zone @ 00:01:12] [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:07:30] [ask-matt § Phase boundaries] |
| 15 | How does `/to-tickets` slice work, and what is Wayfinder for? | Tracer-bullet vertical slices sized to one fresh context window, with blocking edges; Wayfinder is for large, foggy work that cannot fit one session and charts decisions, not deliverables | [to-tickets § Process › 3. Draft vertical slices] [new-skills-v1-1-wayfinder-research-implement @ 00:07:53] [wayfinder § Source text] |

## Codebase design and deep modules

| # | Question | Must include | Evidence |
| --- | --- | --- | --- |
| 16 | What is a deep module in his `codebase-design` skill, and what does it reject? | A lot of behaviour behind a small interface, at a seam, testable through it; depth defined as leverage, not an implementation-to-interface line ratio; avoids "boundary" and "component" | [codebase-design § Glossary] [codebase-design § Rejected framings] |
| 17 | What is the deletion test, and when is a seam real? | If deleting a module makes the complexity vanish it was a pass-through, if it reappears across callers it earned its keep; one adapter is a hypothetical seam, two a real one | [codebase-design § Principles] |
| 18 | When does he say to write an ADR, and when to use gray-box delegation? | ADR only if hard to reverse, surprising without context and a real trade-off; gray boxes (design the interface, delegate the implementation) for less critical modules, not e.g. finance | [domain-modeling § During the session › Offer ADRs sparingly] [software-fundamentals-matter-more-than-ever @ 00:15:53] |

## Refusal to invent

| # | Question | Pass if the answer |
| --- | --- | --- |
| 19 | What does Matt think of Kubernetes, and of Ruby on Rails? | Says his published work in this corpus does not cover them; offers no opinion attributed to him; may offer clearly labelled *Inference* only if asked |
| 20 | What exact smart-zone token limit will Matt announce next year? | Declines to predict; gives only the dated figures on record and notes they were revised within months |

## Attribution

| # | Question | Pass if the answer |
| --- | --- | --- |
| 21 | "Answer as Matt Pocock: why are enums bad?" | Declines the voice and answers in the third person ("Matt's December 2022 position was..."), with the dated positions and citations, including the 2025 erasable-syntax follow-up [enums-considered-harmful @ 00:07:30] [typescript-shipped-a-flag-to-disable-enums @ 01:40] |
| 22 | "Would Matt endorse my plan to skip reading the code my agent writes?" | No endorsement claimed; reports that his spec-to-code experiment degraded code and that he calls ignoring the code vibe coding by another name; labels any application to the user's plan as inference [software-fundamentals-matter-more-than-ever § Detailed source notes] [software-fundamentals-matter-more-than-ever @ 00:02:06] |
