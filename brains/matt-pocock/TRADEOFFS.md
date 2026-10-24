# Tradeoffs

Choices Matt Pocock describes, with the gain, the cost and the condition under which he makes each. Cited; inference is marked. Many entries are positions that he later changed, so the second table lists the reversals with dates. All dates are publication dates of the source, not necessarily when he formed the view.

Synthesis caution: in the cost column, a few short cells (for example "One more agent run" or "Extra tooling") are our plain-language reading of the tradeoff, not his words. The cited sources support the choice and its stated gain, and should not be quoted as his statement of that cost.

## Working with agents

| Choice | Gain | Cost or limit | Source |
| --- | --- | --- | --- |
| Workflow (predetermined steps) over agent (model decides when to stop) | More control; often better for repeated tasks with clear specifications | Agents suit tasks whose steps are unclear; he allows he may be old-fashioned | [what-even-are-agents-and-workflows @ 00:01:06] [what-even-are-agents-and-workflows @ 00:01:44] |
| Clear, hand off or use a subagent instead of compacting | Fresh window; work stays focused | Handoff is for a new harness, a new directory, a colleague or a side task; he dislikes compacting | [ask-matt § Phase boundaries] [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:07:30] |
| Small tasks in fresh windows | Stays out of the dumb zone | A larger advertised window does not remove the limit; it mainly helps retrieval | [what-is-the-dumb-zone @ 00:01:12] [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:36:00] |
| Grill a big task before building | Alignment, so the PRD need not be read closely | Broad scope hides high-fidelity questions and pushes the session into the dumb zone | [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:31:00] [nine-things-wrong-with-grill-skills @ 00:03:43] |
| One-shot prompt for a small diff | Skips alignment overhead | Only for small changes; review the result | [do-you-even-need-human-review @ 00:00:45] |
| Spec as a short-lived schedule, deleted when done | No rotting documents to mislead later agents | Loses a persistent record; he keeps ADRs, a glossary and navigation instead | [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 01:24:00] [delete-most-of-your-docs @ 00:01:45] |
| Spec-to-code without reading the code | Fast | Code degraded with each pass in his experiment | [software-fundamentals-matter-more-than-ever @ 00:00:20] |
| Wayfinder graph over a single spec | Handles fog and multi-session work | Simpler work can go the direct route | [wayfinder-nothing-is-too-big-to-plan-anymore @ 00:08:40] |
| Prototype instead of a long spec | A runnable answer to a design or logic question | Throwaway code, kept only as a source on a throwaway branch | [dont-waste-time-on-specs-prototype-instead @ 00:00:00] [prototype § Rules that apply to both] |
| Gray-box modules: test the boundary, delegate the inside | Less review of implementation | Not for critical modules such as finance; needs a testable boundary | [software-fundamentals-matter-more-than-ever @ 00:15:53] |
| Standards in a separate review sub-agent | Implementer's output is not degraded by extra instructions | One more agent run | [fixing-the-pr-bottleneck-matt-pocock-aihero @ 00:11:30] |
| Refactor in review, not in the TDD loop | Implementation stays focused | Refactoring waits for the review pass | [new-skills-v1-1-wayfinder-research-implement @ 00:11:50] |
| Checks at pre-commit and CI, not per-edit hooks | Less latency and noise | Heavy validation is delayed until CI | [stop-running-your-checks-in-harness-hooks-zgy8_a § Detailed source notes] |
| Hook or lint rule over a CLAUDE.md sentence | Deterministic enforcement | A hook has to be written | [force-claude-code-to-use-the-right-cli @ 00:02:09] [force-claude-code-to-use-the-right-cli @ 00:04:14] |
| Agent memory off | A stateless agent behaves more predictably | Persistent preferences must be written by the user | [kill-your-memorymd @ 00:00:43] [kill-your-memorymd @ 00:01:07] |
| Lower effort level | Fewer thinking tokens; tasks specified better finish at low effort | May need raising for harder tasks | [your-effort-level-is-too-damn-high @ 00:00:09] [your-effort-level-is-too-damn-high @ 00:00:50] |
| Strong model to grill, weaker model to implement | Grilling draws on broad parametric knowledge | Implementation needs enough plan and code context | [nine-things-wrong-with-grill-skills @ 00:08:40] |
| Browser tool for frontend feedback | Supplies visual feedback agents lack | Context cost | [frontend-is-harder-for-ai-than-backend @ 00:00:00] [frontend-is-harder-for-ai-than-backend @ 00:03:53] |
| MCP tools | Any local task can be exposed as a tool | Playwright MCP is context-heavy because of screenshots and tool definitions | [get-started-with-the-model-context-protocol-2-minute-tutorial @ 00:02:00] [frontend-is-harder-for-ai-than-backend @ 00:03:53] |
| Fully automated QA | No human time | He says it produces slop; manual QA imposes taste | [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 01:11:00] |
| Deleting most docs | One source of truth | Rejected alternatives and domain language are not in the code, so ADRs and a glossary stay | [delete-most-of-your-docs @ 00:00:45] [delete-most-of-your-docs @ 00:01:45] |
| Model-invoked skills | The agent can reach for them | Descriptions load into every request and the agent may not follow the pointer | [building-great-agent-skills-the-missing-manual @ 00:03:00] |
| Guardrails you build yourself | More predictable and cheaper, he says | Your own work | [building-llm-apps-that-dont-suck-guardrails § Detailed source notes] |

## Codebase design

| Choice | Gain | Cost or limit | Source |
| --- | --- | --- | --- |
| Deep modules | Leverage for callers and locality for maintainers | A seam and a tested boundary must be designed | [codebase-design § Glossary] |
| Many tiny shallow modules | Looks modular | Hard for the AI to explore; it may not reach the right module | [software-fundamentals-matter-more-than-ever @ 00:13:26] |
| A seam with one adapter | Looks flexible | Hypothetical; add a seam only when something varies across it | [codebase-design § Principles] |
| Extracting pure functions for testability | Easier unit tests | The real bugs can hide in how they are called, a failure of locality | [improve-codebase-architecture § Process › 1. Explore] |
| Tests at the highest seam, ideally one | Fewer structure-sensitive tests | A seam that is too shallow gives false confidence | [to-spec § Process] [diagnosing-bugs § Phase 5: Fix + regression test] |
| ADRs offered sparingly | Only decisions hard to reverse, surprising and a real trade-off | Interchangeable library choices need none | [domain-modeling § During the session › Offer ADRs sparingly] [i-stopped-using-grill-me-for-coding @ 00:07:46] |
| Grill with docs over plain grill-me | Shared terms are saved | Needs a codebase; without one he uses grill-me | [i-stopped-using-grill-me-for-coding @ 00:13:32] |

## TypeScript

| Choice | Gain | Cost or limit | Source |
| --- | --- | --- | --- |
| Generic over `any` or `unknown` | Typed access that follows the input | `any` causes bugs and `unknown` forces manual narrowing | [solving-the-any-problem-is-harder-than-you-think @ 00:03:52] [solving-the-any-problem-is-harder-than-you-think @ 00:04:12] |
| Overloads over a generic | Fits a fixed set of input and output relationships | A generic is simpler when the return type just tracks the input | [overloads-vs-generics-which-is-better § Detailed source notes] |
| An `any` cast inside a generic implementation | Lets a tested public signature carry the contract | Unsafe if the code is not revisited | [zod-generics-is-heaven @ 00:03:00] [typescript-react-and-api-issues-with-matt-pocock @ 00:27:10] |
| Return types omitted on simple functions | Less to maintain; inference updates | Multi-branch functions and libraries should declare them | [should-you-use-return-types @ 00:00:49] [should-you-use-return-types @ 00:01:08] [should-you-use-return-types @ 00:02:32] |
| `satisfies` over an annotation | Keeps narrow types while enforcing a shape | An annotation is right when the variable must accept other keys | [most-ts-devs-dont-understand-satisfies @ 00:01:20] [most-ts-devs-dont-understand-satisfies @ 00:02:50] |
| Value-optional over key-optional | Callers cannot silently drop a value such as a trace ID | A key-optional parameter keeps an API clean when unused in tests | [6-typescript-tips-to-turn-you-into-a-wizard @ 00:00:30] |
| Deriving types from values | A change is made in one place | Coupling across layers, so do not derive props from a database model | [so-many-typescript-devs-make-this-mistake @ 00:00:46] [i-hate-it-when-ts-devs-do-this @ 00:00:32] |
| Narrow cast over suppression directive | A cast affects one node | A directive suppresses everything on the line; `@ts-expect-error` still has uses in tests | [the-typescript-hill-im-willing-to-die-on @ 00:00:43] [typescript-and-career-interview-with-tejas-kumar @ 48:40] |
| Branded types | Catches confusable strings, such as relative versus absolute paths | Verbose; values need `as` assertions | [branded-types-have-saved-my-ass-so-many-times @ 00:01:24] [5-things-theyll-never-add-to-typescript @ 00:04:00] |
| Value objects | Ease of use | A few extra runtime objects | [value-objects-are-so-underused-in-ts @ 00:01:10] |
| Zod only at untrusted edges | No duplicate checks inside the application | Type faith is wrong for a public API | [when-should-you-use-zod § Research note] [sometimes-type-faith-is-enough @ 00:00:26] |
| Augmenting `process.env` | Editor autocomplete | Does not validate real values; values are always strings | [how-to-strongly-type-process-env § Research note] [youre-typing-process-env-wrong @ 02:03] |
| `NodeNext` when tsc emits; `bundler` when something else runs the code | Output Node can consume, or bundler-style resolution | Under `NodeNext` imports need `.js` extensions | [the-tsconfig-cheat-sheet @ 00:02:30] [the-tsconfig-cheat-sheet @ 00:02:50] [js-files-in-typescript-why @ 00:00:31] |
| Monorepo | Front end and back end deploy as one unit; types shared | Extra tooling | [coding-road-trip-driving-truck-typescript @ 03:30] |
| Codegen and Prisma-style tools | Prevent a class of bugs | Types do not reach the editor until codegen reruns | [codegen-the-good-the-bad-and-the-ugly @ 04:31] [codegen-the-good-the-bad-and-the-ugly @ 05:44] |
| Complex template literal and mapped types | Expressive types for flexible JavaScript | Users can spend the compiler's speed gain on complexity, a warning from Anders Hejlsberg | [has-typescript-gone-too-far @ 00:00:05] [live-anders-hejlsberg-typescript-go-port @ 00:45:00] |
| Strict mode on | Libraries assume it | Off only when migrating a very large JavaScript codebase | [the-tsconfig-cheat-sheet @ 00:01:30] [typescript-speedrun-crash-course-for-beginners @ 00:08:13] |
| Polymorphic button-or-link component | One component | Asks whether the design is needed; otherwise split into separate components | [rethink-your-life-choices-before-you-do-this @ 00:03:15] |

## Conditions behind the choices

- Application, library and shared code get different rules. [application-vs-library-ts-typescript § Detailed source notes]
- Trust at the input boundary decides where to validate. [when-should-you-use-zod § Research note]
- The size of the diff decides how much alignment and review are needed. [do-you-even-need-human-review @ 00:01:41]
- Context pressure decides where standards go: implementation has the most and review the least. [retro § Reference › Implementation vs Review]

## Positions that changed

| Topic | Earlier | Later | Source |
| --- | --- | --- | --- |
| Types versus interfaces | 2022: interface for performance, then `type` unless an interface feature is needed | 2025: the performance claim conflated interfaces with intersections; prefer `interface extends` over intersections | [typescript-types-or-interfaces-deprecated-2022 § Detailed source notes] [types-vs-interfaces-what-i-got-wrong-in-2022 @ 00:00:44] |
| ChatGPT | March 2023: less useful than it seems for real programming | Agents became useful to him around the end of 2025 | [total-typescript-tour-and-ama @ 00:00:14] [matt-pocock-the-most-valuable-skill-in-the-age-of-ai @ 00:00:01] |
| Vibe coding | May 2025: code is cheap, so deleting a feature and rebuilding may beat debugging | April 2026: discarding code behind specs is a mistake; the code is the environment | [vibe-coders-wont-be-able-to-fix-bugs @ 00:00:27] [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:13:00] |
| Agents versus workflows | June 2025: "agents are dead, long live workflows" | September 2025 definitions; by 2026 unattended loops over a queue of scoped tasks | [agents-are-dead-long-live-workflows @ 00:00:35] [what-even-are-agents-and-workflows @ 00:00:10] [matt-pocock-s-agentic-engineering-workflow-just-copy-him @ 00:42:00] |
| Planning | Multi-phase plans until late 2025; January 2026 plan mode | Grilling plus specs and tickets, then Wayfinder in July 2026 | [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:05:00] [i-was-an-ai-skeptic-then-i-tried-plan-mode @ 00:00:00] [new-skills-v1-1-wayfinder-research-implement @ 00:07:53] |
| Smart-zone threshold | About 100K tokens in April 2026 | About 120K in May and about 150K in July | [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:03:00] [nine-things-wrong-with-grill-skills @ 00:03:43] [what-is-the-dumb-zone @ 00:00:53] |
| Grill-me cadence | One question at a time in version 1.1 | Rounds of dependent questions, then the whole ready frontier per round | [new-skills-v1-1-wayfinder-research-implement @ 00:00:37] [this-change-makes-grill-me-so-much-better @ 00:01:01] [grilling § Source text] |
| Grilling in a codebase | Plain `/grill-me` | `/grill-with-docs` from May 2026 | [i-stopped-using-grill-me-for-coding @ 00:00:00] |
| PRD and issues | PRD and issue skills | Renamed spec and tickets in July 2026; CONTEXT.md became GLOSSARY.md in v1.3 | [new-skills-v1-1-wayfinder-research-implement @ 00:00:37] [skills-changelog-v13 § Factual notes] |
| Instruction files | April 2026: always-applying standards in CLAUDE.md | June 2026: delete them as a reset, then layer back what is needed | [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 01:24:00] [matt-pocock-s-agentic-engineering-workflow-just-copy-him @ 00:56:00] |
| Checks in harness | February 2026: a hook to enforce a CLI rule | October 2026: do not run checks in per-edit hooks | [force-claude-code-to-use-the-right-cli @ 00:02:09] [stop-running-your-checks-in-harness-hooks-zgy8_a § Detailed source notes] |
| `/init` | February 2026: never run it | March 2026: more positive about the smaller output but still prefers a compact file and a skill | [never-run-claude-init @ 00:00:00] [claude-code-tried-to-improve-init @ 00:05:05] |
| Effort | June 2026: medium effort | July 2026: start at the lowest | [matt-pocock-s-agentic-engineering-workflow-just-copy-him @ 00:24:00] [your-effort-level-is-too-damn-high @ 00:00:50] |
| MCP | March 2025: a two-minute tutorial | June 2026: delete MCP servers as a reset | [get-started-with-the-model-context-protocol-2-minute-tutorial @ 00:00:12] [matt-pocock-s-agentic-engineering-workflow-just-copy-him @ 00:56:00] |
| TDD refactor step | Refactor inside the loop | July 2026: refactoring moved into code review | [new-skills-v1-1-wayfinder-research-implement @ 00:11:50] |
| Return types | 2023: leave off simple functions | A late-2023 clip on AI argues for an explicit return type | [should-you-use-return-types @ 00:00:49] [will-ts-or-js-win-our-ai-future @ 00:00:18] |
| Zod alternatives | April 2023: ArkType not yet recommended over Zod | June 2025: ArkType a solid option | [everyones-talking-about-arktype @ 00:02:00] [open-source-libraries-you-have-to-know-arktype @ 01:28] |

## Applying this

- *Inference:* when a source gives a threshold, tool preference or skill behaviour, find its date in the tables above before relying on it, since he revised several within weeks.
- *Inference:* a rule given for one audience (application, library, small diff, critical module) should not be applied to another without saying so.
