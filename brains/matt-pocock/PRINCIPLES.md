# Principles

How Matt Pocock reasons about TypeScript, AI-assisted programming and teaching. Each principle is what the sources say, cited; *Synthesis:* lines draw one conclusion across several cited passages; *Applying this* lines are ours, not his. Most video evidence is reviewer-written source notes of auto-captioned talks, so a cited sentence is a note-taker's report of what he said ([SOURCES.md](SOURCES.md#weighting)). His positions moved often, so dates are given and [TRADEOFFS.md](TRADEOFFS.md) lists the reversals.

## 1. The code is the environment the agent works in

He argues that software fundamentals matter more with AI, because the codebase is the environment the agent operates in. [do-software-fundamentals-still-matter § Detailed source notes] His evidence is an experiment with spec-driven development, where the spec was edited and the compiler re-run without reading the code, and each pass produced worse code. [software-fundamentals-matter-more-than-ever § Detailed source notes] He calls ignoring the code "vibe coding by another name". [software-fundamentals-matter-more-than-ever @ 00:02:06]

He compares an agent to the amnesiac in Memento: each session starts by re-learning the codebase, so how explorable the codebase is matters more than model strength. [do-software-fundamentals-still-matter @ 00:01:10] He says the codebase influences the output more than the prompts or the AGENTS.md file. [your-codebase-is-not-ready-for-ai @ 00:00:03]

*Applying this:* before tuning prompts, look at the repository the agent has to navigate. Structure, names and test seams are part of the prompt.

## 2. Agents are tactical; the developer supplies the strategy

He borrows Ousterhout's split and says agents are good tactical programmers and weak at strategy, partly because they are rewarded on tactical benchmarks. [tell-your-agents-to-care-about-the-codebase @ 00:00:00] The developer has to be the annoying colleague who asks for the refactor first, without reading every line. [tell-your-agents-to-care-about-the-codebase @ 00:00:45] Once told the codebase should improve over time, the agent cares and does strategic work well. [tell-your-agents-to-care-about-the-codebase @ 00:01:45]

He frames the agent as a "tactical tornado" unless steered. [your-agent-is-a-tactical-tornado-vo33ex § Detailed source notes] The note records his reading of incentives as his own reading, not a measurement. [tell-your-agents-to-care-about-the-codebase § Detailed source notes]

## 3. Design the interface, delegate the implementation

He recommends deep modules, with small interfaces hiding substantial behaviour, as easier for AI to navigate and test. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 01:14:00] The human designs and verifies the interface and the agent handles what is behind it, which he calls a gray box. [your-codebase-is-not-ready-for-ai @ 00:03:03] He attaches conditions: it is for less critical modules, since he gives finance as a case where he would still review the implementation, and it needs a testable boundary outside the module. [software-fundamentals-matter-more-than-ever @ 00:15:53]

It does not remove taste: he says it is still a million miles away from vibe coding because the human decides what goes into each module. [your-codebase-is-not-ready-for-ai @ 00:05:50]

*Applying this:* delegation is safest where a boundary can be tested from outside and the cost of a wrong implementation is low.

## 4. Context is finite, so work in small fresh pieces

Output quality drops as the context fills, because each new token must relate to every other. [what-is-the-dumb-zone @ 00:00:06] Performance sinks gradually rather than at a single cliff. [what-is-the-dumb-zone @ 00:00:31] His working answer is to break work into small chunks that run in fresh windows. [what-is-the-dumb-zone @ 00:01:12] A larger advertised window does not change this: he says a one-million-token window mainly helps retrieval. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:36:00]

He attributes the smart-zone and dumb-zone framing to Dex Horthy and treats the boundary as a rule of thumb, not a hard cutoff. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:03:00] [agent-skills-with-matt-pocock § Detailed source notes] He moved his own estimate upward over 2026. [what-is-the-dumb-zone § Detailed source notes]

*Synthesis:* the same concern about a large always-loaded footprint recurs for tool definitions, long rules, harness prompts and skills, so his tool opinions become more conditional on context cost. [stop-giving-your-llm-more-tools § Detailed source notes] [claude-codes-system-tools-are-so-bloated § Detailed source notes] [your-skills-are-way-too-long-y81cje § Detailed source notes]

## 5. Align before building, and give the project a shared language

He treats the grilling interview as the way to reach a shared, unwritten theory of what is being built before planning. [software-fundamentals-matter-more-than-ever @ 00:03:40] Because the grilling creates the alignment, he does not usually read the PRD closely afterwards. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:31:00]

Inside a codebase the agent kept asking what local terms meant, and the language found in a session was not saved, so he moved to grilling combined with a glossary built on domain-driven design's ubiquitous language. [i-stopped-using-grill-me-for-coding § Detailed source notes] He argues DDD's terms suit AI because they are already in the model's training, and says the claimed benefits are observations, not measured outcomes. [senior-dev-this-grill-me-prompt-is-going-viral-among-top-engineers § Detailed source notes] [software-fundamentals-matter-more-than-ever § Detailed source notes]

## 6. Keep humans at the checkpoints that need taste

He does not want every checkpoint automated at once. For big work that needs alignment, align early and still review before production. [do-you-even-need-human-review @ 00:00:26] For small changes, skip alignment and review the result. [do-you-even-need-human-review @ 00:00:45] The rule is to shift checkpoints left for big work and right for small work whose diff is easy to regenerate. [do-you-even-need-human-review @ 00:01:41]

Manual QA is where taste is imposed, and he says fully automated creation, QA and research produce slop. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 01:11:00]

## 7. The quality of the feedback loop caps the quality of the output

He says an agent's output is capped by the quality of its feedback loops. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 01:08:00] Types, tests and a fast CI are the constraints that keep an agent on course. [most-companies-are-not-ready-for-background-agents @ 00:01:03]

He worries that checks can lie, with examples such as tests that restate the implementation and tests that cannot fail because an API was stubbed out. [fixing-the-pr-bottleneck-matt-pocock-aihero @ 00:03:00] His answers are test-first work, so that the agent's work is legible, and mutation testing, whose surviving mutants give the agent rich feedback to write stronger tests. [red-green-refactor-is-op-with-claude-code @ 00:02:00] [mutation-testing-is-awesome @ 00:00:16] [mutation-testing-is-awesome @ 00:00:31]

## 8. Implementation and review belong in separate contexts

He says standards placed in the implementer's instructions degrade its output, because implementation is already heavy; review should be a separate skill run in a sub-agent that reads a team-owned standards file. [fixing-the-pr-bottleneck-matt-pocock-aihero @ 00:11:30] The retro skill gives the reason: implementation has the most context pressure and review the least, so review should impose the standards. [retro § Reference › Implementation vs Review]

He frames the PR bottleneck with three brakes, automated checks, automated agent review and human review. [fixing-the-pr-bottleneck-matt-pocock-aihero @ 00:02:00]

## 9. Enforce mechanically; keep instructions sparse and single-sourced

A global CLAUDE.md cannot deterministically stop a command, so for enforcement he asks for a pre-tool-use hook, and for style he suggests ESLint rules. [force-claude-code-to-use-the-right-cli @ 00:02:09] [force-claude-code-to-use-the-right-cli @ 00:04:14] He says never to run `/init`, since oversized or stale guidance consumes tokens and most of it can be discovered from the repo. [never-run-claude-init @ 00:00:00] [never-run-claude-init @ 00:03:08]

His rule for where guidance lives is push versus pull: standards that always apply go in CLAUDE.md, while skills are pulled by the agent when needed. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 01:24:00] He says to delete most docs that restate code, because they drift and give the agent two sources of truth, and keeps decision records for rejected alternatives, a glossary and a thin navigation layer. [delete-most-of-your-docs @ 00:00:45] [delete-most-of-your-docs @ 00:01:45]

## 10. Skills are procedures the user runs

He separates procedures the human runs from abilities the model invokes, and his own set leans user-invoked because model-invoked skills add descriptions to every request. [matt-pocock-s-agentic-engineering-workflow-just-copy-him @ 00:17:00] [building-great-agent-skills-the-missing-manual @ 00:03:00] He says skills should empower the user, and that piling many skill frameworks into one agent overloads the model. [framework-hell-tutorial-hell-now-skill-hell § Detailed source notes]

His writing guide treats a skill description or an AGENTS.md line as a context pointer, whose wording decides when the agent reaches the material. [writing-for-agents § Context pointers] It counts two loads: context load on the agent's window and cognitive load on the human. [writing-for-agents § The two loads]

## 11. Communication is the bottleneck

He argues that people underestimate the communication gap between themselves and an agent, and places the root of the gap in people rather than in the model. [dont-underestimate-the-communication-barrier § Detailed source notes] He says a skilled communicator with an older model can outperform someone who cannot communicate with a newer one. [matt-pocock-the-most-valuable-skill-in-the-age-of-ai § Detailed source notes]

## 12. TypeScript is tooling first

He argues that TypeScript is mainly about better tooling rather than adding types to JavaScript as such. [typescript-vs-javascript @ 00:00:33] He frames it as an editor layer of autocomplete, go-to-definition, inline errors and safe rename refactors, with types erased at runtime. [dejavue-e023-typescript-and-content-creation-with-matt-pocock @ 00:14:00] He judges tools by feedback: time to error was the first benefit in his migration pitch. [why-you-should-migrate-to-typescript § Detailed source notes]

## 13. Ask who the advice is for

He separates application code, library code and the shared code inside an application, and says rules suited to a library are not the rules for application leaves. [application-vs-library-ts-typescript § Detailed source notes] On a debate stream he split `any` the same way: enable the lint rule in application code with rare documented exceptions, allow it in library code and test suites. [ending-every-typescript-debate-in-90-minutes @ 01:08:00] He says the complexity of generics, `extends` and `infer` is mostly library authors' work. [to-typescript-or-not-to-typescript-jsj-538 @ 00:17:00]

## 14. Trust decides where to validate

Whether to validate at runtime depends on trust: untrusted external values should be validated and values controlled inside the application may not need duplicate checks. [when-should-you-use-zod § Research note] He accepts a little "type faith" when his team controls the API and rejects it for a public API that malicious users can reach. [sometimes-type-faith-is-enough @ 00:00:26]

## 15. One source of truth, narrow over wide

Closely related types should be derived from one another so a change is made in one place. [so-many-typescript-devs-make-this-mistake @ 00:00:46] The counter-rule applies across layers: do not derive a component's prop type from a database model, because it couples the front end to the data shape. [i-hate-it-when-ts-devs-do-this @ 00:00:32] [i-hate-it-when-ts-devs-do-this @ 00:00:45]

On escape hatches he prefers the narrowest tool: a cast affects one node, while a suppression directive covers the whole line and cannot name the error it expects. [the-typescript-hill-im-willing-to-die-on @ 00:00:43] [the-typescript-hill-im-willing-to-die-on @ 00:01:51]

## 16. Teach from the point where the learner is stuck

His lesson pattern is to present a task, let the learner attempt it, run an automated check, then show his own solution and discuss tradeoffs. [course-creation-interview-with-cory-house § Detailed source notes] His rationale is that presenting information at the moment a learner is stuck creates a useful slot for the explanation; the note flags this as his teaching rationale, not independent learning-science evidence. [course-creation-interview-with-cory-house § Detailed source notes]

He models course knowledge as a directed acyclic graph of prerequisites. [course-creation-interview-with-cory-house § Detailed source notes] His explanations open with the failing code and then the fix. [string-literals-are-great @ 00:00:11] He splits one fuzzy word into distinct operations, for example "generics" into three. [typescript-generics-in-three-easy-patterns § Research note]

## 17. He revises in public and labels his own uncertainty

He corrected his 2022 types-versus-interfaces performance claim in 2025, saying the TypeScript wiki contrasted interfaces with intersections. [types-vs-interfaces-what-i-got-wrong-in-2022 @ 00:00:44] On Valibot he said he had not tested it. [everyones-talking-about-valibot @ 00:03:45] He frames contested calls, such as suppression directives, as his own preferences rather than universal policy. [typescript-and-career-interview-with-tejas-kumar @ 48:40]

*Applying this:* treat any single video as a dated snapshot. When two sources disagree, prefer the later one and say that the position changed.
