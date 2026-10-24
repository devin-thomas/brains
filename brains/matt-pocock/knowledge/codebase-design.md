---
brain: matt-pocock
topic: codebase-design
sources: [codebase-design, improve-codebase-architecture, domain-modeling, grill-with-docs, ask-matt, tdd, to-spec, diagnosing-bugs, code-review, skills-changelog-v13, software-fundamentals-matter-more-than-ever, how-to-de-slop-a-codebase-ruined-by-ai, your-codebase-is-not-ready-for-ai, senior-dev-this-grill-me-prompt-is-going-viral-among-top-engineers, i-stopped-using-grill-me-for-coding, ai-skills-with-matt-pocock, fixing-the-pr-bottleneck-matt-pocock-aihero, full-walkthrough-workflow-for-ai-coding-matt-pocock, live-uncle-bob-software-fundamentals-ai, delete-most-of-your-docs, do-software-fundamentals-still-matter, there-is-no-such-thing-as-greenfield, tell-your-agents-to-care-about-the-codebase, your-agent-is-a-tactical-tornado-vo33ex, five-claude-code-skills-i-use-every-day, building-a-real-feature-with-claude-code, matt-pocock-s-agentic-engineering-workflow-just-copy-him, matt-pocock-the-most-valuable-skill-in-the-age-of-ai, new-skills-v1-3-pr-implement-spec-retro, never-run-claude-init, the-secret-to-building-robust-frontend-apps-with-agents, modelling-statecharts-step-by-step, application-vs-library-ts-typescript]
drafted_by: sonnet
reviewed: 2026-10-24 by sonnet lead pass (citation check with tools/check_citations.py; claims spot-checked against corrected transcripts; not every claim re-read against its source)
---

# Codebase design and architecture, per Matt Pocock

Matt Pocock's position on codebase design is that the code is the environment an AI agent works in, so the quality of its structure decides the quality of the agent's output [do-software-fundamentals-still-matter § Detailed source notes]. His concrete answer is to build a codebase out of deep modules, test at the seams between them, give the project a shared domain language in a glossary, and record only the hard-to-reverse decisions as ADRs [software-fundamentals-matter-more-than-ever § Detailed source notes]. He packages this as four skills in his public skills repository: `codebase-design` (the vocabulary), `improve-codebase-architecture` (the survey), `domain-modeling` (the glossary and ADR discipline) and `tdd` (tests at agreed seams) [ask-matt § Codebase health] [ask-matt § Vocabulary underneath] [tdd § Seams: where tests go].

Two notes on the evidence. The material is almost entirely from 2026: before February 2026 the corpus has only statechart, type-placement and shared-code remarks (see "Changed over time"). And it comes in two registers: skill files (MIT, full text) say what the skills instruct, while video notes and corrected transcripts record why. A fact taken from a corrected transcript beyond a note's text is cited as `[slug @ time]`.

## Why design matters more with agents

Matt's thesis is that software fundamentals matter more with AI, not less, because bad code is the most expensive it has ever been when an agent has to work in a codebase that is hard to change [software-fundamentals-matter-more-than-ever § Detailed source notes]. He reached it after trying the "specs to code" approach, where the spec is edited and the compiler re-run without reading the code, and finding that each pass produced progressively worse code [software-fundamentals-matter-more-than-ever § Detailed source notes]. He calls ignoring the code "vibe coding by another name" [software-fundamentals-matter-more-than-ever @ 00:02:06]. He tells the same story on the Pragmatic Engineer podcast, and in a later talk says that as the specs were refined for a custom video editor the code degraded and the agent's output worsened over months [ai-skills-with-matt-pocock @ 00:51:33] [matt-pocock-the-most-valuable-skill-in-the-age-of-ai § Detailed source notes].

He borrows three framings from older books to explain what he saw.

- **Complexity** is John Ousterhout's term for any structure that makes a system hard to understand and modify, so a bad codebase is one that is hard to change [software-fundamentals-matter-more-than-ever § Detailed source notes].
- **Software entropy** comes from The Pragmatic Programmer: each change made without regard for the whole system makes the codebase worse [software-fundamentals-matter-more-than-ever § Detailed source notes]. Matt says AI has accelerated entropy, so codebases fall apart faster than before [how-to-de-slop-a-codebase-ruined-by-ai § Detailed source notes].
- **Tactical versus strategic programming** is also Ousterhout's. Tactical work gets the current task working; strategic work invests in design so the next change is easier [your-agent-is-a-tactical-tornado-vo33ex § Detailed source notes]. Matt says agents behave like Ousterhout's "tactical tornado", a fast developer who leaves a mess behind, so the human has to supply the strategic view [your-agent-is-a-tactical-tornado-vo33ex § Detailed source notes].

Why agents are tactical, in his account: they are trained and rewarded on tactical benchmarks, and the harness is unlikely to push them toward refactoring first, though he flags this as his reading of incentives [tell-your-agents-to-care-about-the-codebase § Detailed source notes]. His remedy is for the developer to play the "annoying colleague" who asks for a refactor before new work, without reading every line, and to tell the agent the codebase should improve over time, after which it cares and does strategic work well [tell-your-agents-to-care-about-the-codebase § Detailed source notes].

Agents also differ from humans in memory. A human can work around a bad codebase by remembering what works; an agent has no memory and must be re-taught each session, like the amnesiac in the film Memento [do-software-fundamentals-still-matter § Detailed source notes]. He says this holds for weaker and stronger models alike, because the difficulty is exploring and changing the code, not reasoning [do-software-fundamentals-still-matter § Detailed source notes]. On the podcast he calls the practice "Memento-driven development" [ai-skills-with-matt-pocock @ 01:00:14].

He ranks the codebase above the other levers: it influences output more than the prompts or the AGENTS.md file, and a poor design costs slow feedback, trouble finding files and working out how to test, and cognitive burnout for the human [your-codebase-is-not-ready-for-ai @ 00:00:03]. Likewise the harness (prompts, skills, environment and the codebase) matters more than the model, and a clean codebase lets cheaper models and fewer tokens do the work [matt-pocock-s-agentic-engineering-workflow-just-copy-him § Detailed source notes].

Strategic skill is hard to learn because its feedback loop is long: on the podcast he compares strategic decisions to mixing-desk sliders whose effects you hear months later, and says AI shortens the loop only by letting you hit the consequences sooner [ai-skills-with-matt-pocock @ 01:01:31]. He recommends A Philosophy of Software Design, the first chapters of Domain-Driven Design, and The Pragmatic Programmer [ai-skills-with-matt-pocock § Detailed source notes].

## The shared vocabulary: the `codebase-design` skill

The skill opens by stating the goal: deep modules, meaning a lot of behaviour behind a small interface, placed at a clean seam and testable through that interface, for the sake of leverage for callers, locality for maintainers and testability for everyone [codebase-design § Glossary]. It tells the agent to use its terms exactly and not to substitute "component", "service", "API" or "boundary" [codebase-design § Glossary]. Matt's reason, given in a talk on review and coding standards, is that anyone who reads up on structuring a codebase finds many approaches that all get called DDD, so a team needs one consistent language [fixing-the-pr-bottleneck-matt-pocock-aihero @ 00:10:18]. The definitions are:

- **Module**: anything with an interface and an implementation, deliberately scale-agnostic, so a function, class, package or a slice that spans tiers all count. The skill says to avoid "unit", "component" and "service" for it [codebase-design § Glossary]. In the de-slop video Matt gives examples ranging from a set of React components forming a page to the authentication functions to a logger [how-to-de-slop-a-codebase-ruined-by-ai § Detailed source notes].
- **Interface**: everything a caller must know to use the module correctly, which includes invariants, ordering constraints, error modes, required configuration and performance characteristics as well as the type signature. The skill says to avoid "API" and "signature" because they are too narrow [codebase-design § Glossary]. In the video he adds that it includes nebulous usage information such as documentation [how-to-de-slop-a-codebase-ruined-by-ai @ 00:02:22].
- **Implementation**: what is inside the module. The skill distinguishes it from an adapter: a small adapter can have a large implementation (a Postgres repository) and a large adapter can have a small implementation (an in-memory fake) [codebase-design § Glossary].
- **Depth**: leverage at the interface, meaning how much behaviour a caller or test can exercise per unit of interface they must learn. A deep module has a large amount of behaviour behind a small interface; a shallow one has an interface nearly as complex as its implementation [codebase-design § Glossary].
- **Seam**, credited to Michael Feathers: a place where behaviour can be altered without editing in that place, and the location where a module's interface lives. Where to put the seam is its own design decision, separate from what goes behind it, and the skill says to avoid "boundary" because DDD already uses it for bounded context [codebase-design § Glossary].
- **Adapter**: a concrete thing that satisfies an interface at a seam. It describes a role, the slot it fills, not what is inside it [codebase-design § Glossary].
- **Leverage** is what callers get from depth: more capability per unit of interface, so one implementation pays back across many call sites and tests [codebase-design § Glossary].
- **Locality** is what maintainers get from depth: change, bugs, knowledge and verification concentrate in one place instead of being spread across callers [codebase-design § Glossary].

It also lists framings it rejects. It does not define depth as a ratio of implementation lines to interface lines (Ousterhout's framing), because that rewards padding the implementation, and uses depth-as-leverage instead. It rejects reading "interface" as the TypeScript keyword or a class's public methods, and it rejects "boundary" [codebase-design § Rejected framings]. This is a deliberate departure from the source book's wording, even though Matt credits Ousterhout for the deep-versus-shallow idea itself [how-to-de-slop-a-codebase-ruined-by-ai § Detailed source notes].

## Deep modules, shallow modules and navigability

The skill's picture of a deep module is a small interface (few methods, simple parameters) over a deep implementation that hides complex logic, and of a shallow module a large interface over a thin implementation that just passes calls through [codebase-design § Deep vs shallow]. When designing an interface it prompts three questions: can the methods be reduced, can the parameters be simplified, can more complexity be hidden inside [codebase-design § Deep vs shallow]. Matt's quality example of a deep module is a well-regarded open-source library such as TanStack Query, which hides a lot behind a simple interface [how-to-de-slop-a-codebase-ruined-by-ai § Detailed source notes].

The principles in the skill add four rules.

- **Depth belongs to the interface, not the implementation.** A deep module may be built from small, mockable, swappable parts as long as they are not part of the interface, and a module can have internal seams used by its own tests as well as the external seam at its interface [codebase-design § Principles].
- **The deletion test.** Imagine deleting the module: if the complexity vanishes it was a pass-through, and if the complexity reappears across many callers it was earning its keep [codebase-design § Principles].
- **The interface is the test surface.** Callers and tests cross the same seam, so wanting to test past the interface suggests the module is the wrong shape [codebase-design § Principles].
- **One adapter means a hypothetical seam; two adapters means a real one.** The skill says not to introduce a seam unless something actually varies across it [codebase-design § Principles].

Why this helps agents is Matt's most repeated argument, and he makes it in several places.

1. **Navigation.** In his talk on software fundamentals he says a codebase made of many tiny shallow modules is hard for the AI to explore; it may not reach the right module or understand the dependencies [software-fundamentals-matter-more-than-ever @ 00:13:26]. In "Your codebase is NOT ready for AI" he says the file system should match the mental map the developer holds, because an agent starts with no memory and sees only a jumble of exports [your-codebase-is-not-ready-for-ai § Detailed source notes]. He wants groups of modules in discoverable folders with the public types visible, so the agent reads the interface first and opens the implementation only if it needs to, which he calls progressive disclosure [your-codebase-is-not-ready-for-ai § Detailed source notes].
2. **Testing.** Deep modules mean fewer structure-sensitive tests, because more of the implementation is hidden behind the interface and tests can target only the interface [fixing-the-pr-bottleneck-matt-pocock-aihero § Detailed source notes]. His job as the human, he says, is to force the agents to use the small interface rather than reach into implementation details [fixing-the-pr-bottleneck-matt-pocock-aihero @ 00:09:10].

He presents this as old advice reapplied, not a new AI method [your-codebase-is-not-ready-for-ai § Detailed source notes]. Terms from the engineering literature act as "leading words": they are in the model's training, so naming them in a prompt makes the model use them back, as with "tracer bullet", and deep modules became a massive one for him [ai-skills-with-matt-pocock @ 00:55:19].

## Gray boxes: design the interface, delegate the implementation

The practical payoff Matt draws from deep modules is a division of labour. The human designs and verifies the interface, the agent handles what is behind it, and tests lock the behaviour down [your-codebase-is-not-ready-for-ai § Detailed source notes]. He calls such modules gray boxes: the implementation can still be opened for performance or taste, but routine changes rely on the boundary and the tests [your-codebase-is-not-ready-for-ai § Detailed source notes]. The slogan in the AI Engineer talk is "design the interface, delegate the implementation" [software-fundamentals-matter-more-than-ever § Detailed source notes].

The conditions he attaches are specific.

- The gray-box approach is for less critical modules. He gives finance as an example where he would not skip reviewing the implementation [software-fundamentals-matter-more-than-ever @ 00:15:53].
- It needs a testable boundary outside the module, and the human must understand the module's purpose well enough to design it from outside [software-fundamentals-matter-more-than-ever @ 00:15:53].
- It does not remove taste: it is "still a million miles away" from vibe coding because the human must decide what goes into each module and design the boundaries [your-codebase-is-not-ready-for-ai @ 00:05:50].
- Module boundaries belong in the shared vocabulary and in the PRD, where he states which modules change and how their interfaces change [software-fundamentals-matter-more-than-ever § Detailed source notes].

He quotes Kent Beck's principle of investing in the design of the system every day, and says the specs-to-code approach does the opposite and divests from design [software-fundamentals-matter-more-than-ever § Detailed source notes] [software-fundamentals-matter-more-than-ever @ 00:16:41].

In a May 2026 interview the host raised the "narrow waist" idea (fix what goes into and out of a module, let the inside vary, so slop is contained), and the note records Matt connecting it to deep modules and ports-and-adapters [senior-dev-this-grill-me-prompt-is-going-viral-among-top-engineers § Detailed source notes]. The framing was the host's, not Matt's coinage [senior-dev-this-grill-me-prompt-is-going-viral-among-top-engineers @ 00:04:35].

## Testability and seams

The skill gives three rules for testable interfaces: accept dependencies instead of creating them, return results instead of producing side effects, and keep the surface area small, because fewer methods means fewer tests and fewer parameters means simpler setup [codebase-design § Designing for testability]. Its examples contrast passing a payment gateway into `processOrder` with constructing a Stripe gateway inside it, and a `calculateDiscount` that returns a discount with an `applyDiscount` that mutates the cart [codebase-design § Designing for testability].

Matt says testing is hard because it forces interdependent decisions (unit size, what to mock, which behaviours matter), and that good codebases are easy to test, so a better codebase gives the agent better feedback [software-fundamentals-matter-more-than-ever @ 00:12:12]. Deep modules are his answer: test at the interface and verify through it [software-fundamentals-matter-more-than-ever @ 00:14:40].

The skills turn this into process rules about seams.

- **`tdd`:** tests live at seams, never against internals, and a seam is the public boundary where behaviour is observed without reaching inside [tdd § Seams: where tests go]. The skill requires writing down the seams under test and confirming them with the user before any test is written, with a one-line note on what each seam catches and misses [tdd § Seams: where tests go]. When the shape of the interface itself is in question, it points to `codebase-design` as a reference to consult [tdd § Seams: where tests go].
- **`tdd` anti-patterns:** implementation-coupled tests (mocking internal collaborators, testing private methods, or checking through a side channel such as querying the database instead of using the interface), and tautological tests whose expected value is recomputed the way the code does it [tdd § Anti-patterns]. The tell for the first is a test that breaks on a refactor when behaviour has not changed [tdd § Anti-patterns].
- **`to-spec`:** the agent sketches the seams at which the feature will be tested, prefers existing seams, uses the highest seam possible, and proposes new seams at the highest point it can. The skill states that fewer seams is better and the ideal number is one, and that the user must confirm them [to-spec § Process].
- **`diagnosing-bugs`:** a regression test is written before the fix only if there is a correct seam, meaning one where the test exercises the real bug pattern as it occurs at the call site [diagnosing-bugs § Phase 5: Fix + regression test]. If the only seam is too shallow, such as a single-caller test when the bug needs several callers, the test gives false confidence [diagnosing-bugs § Phase 5: Fix + regression test]. If no correct seam exists, the skill says that is itself the finding, because the architecture is preventing the bug from being locked down, and it is flagged for the next phase [diagnosing-bugs § Phase 5: Fix + regression test].

In the review talk Matt shows how structure-coupled tests arise: a test stubs the DOM audio context with fake methods, so the real API's error modes can never fail it, and he asks how to make automated checks harder to cheat, even in good faith [fixing-the-pr-bottleneck-matt-pocock-aihero @ 00:07:30]. His first answer is codebase design, since deeper modules leave fewer structure-sensitive checks [fixing-the-pr-bottleneck-matt-pocock-aihero § Detailed source notes]. His second is to keep coding standards out of the overloaded implementing agent and apply them in a separate review sub-agent that reads a standards file [fixing-the-pr-bottleneck-matt-pocock-aihero § Detailed source notes].

The code-review skill carries design smells tied to these ideas: Shotgun Surgery (gather what changes together into one module), Divergent Change (split a module edited for unrelated reasons), Middle Man and Primitive Obsession, each a judgement call that a repo's documented standard can override [code-review § Process › 3. Identify the standards sources].

In an October 2026 short he praises reducers for front-end work with agents: the reducer takes an action and returns the next state, can be tested outside the browser, and keeps the React code simple [the-secret-to-building-robust-frontend-apps-with-agents § Detailed source notes].

## Improving architecture: the `improve-codebase-architecture` skill

The skill's job is to surface architectural friction and propose "deepening opportunities", refactors that turn shallow modules into deep ones, for the sake of testability and AI-navigability. It is informed by the project's domain model and built on the `codebase-design` vocabulary [improve-codebase-architecture § Source text]. It is marked as user-invoked only, with model invocation disabled [improve-codebase-architecture § Original source frontmatter].

**Step one, explore.** The skill starts with a YAGNI scoping rule: since deepening pays off by making future changes easier, weight the parts of the codebase that changed recently. If the user named a direction, take it; otherwise read the commit history to find hot spots and widen the net if changes are scattered [improve-codebase-architecture § Process › 1. Explore]. It reads `GLOSSARY.md` and any ADRs in the area first, then spawns a sub-agent to walk the codebase, noting friction and not following rigid heuristics [improve-codebase-architecture § Process › 1. Explore]. The friction questions it asks are:

- where does understanding one concept require bouncing between many small modules;
- where is a module shallow, with an interface nearly as complex as its implementation;
- where have pure functions been extracted just for testability while the real bugs hide in how they are called, which is a failure of locality;
- where do tightly-coupled modules leak across their seams;
- which parts are untested or hard to test through their current interface [improve-codebase-architecture § Process › 1. Explore].

Anything suspected of being shallow gets the deletion test: would deleting it concentrate complexity or just move it [improve-codebase-architecture § Process › 1. Explore].

**Step two, report.** The agent writes a self-contained HTML report to the OS temp directory so nothing lands in the repo, and opens it [improve-codebase-architecture § Process › 2. Present candidates as an HTML report]. Each candidate card has files, problem, solution, benefits (in locality and leverage and test improvement), a before/after diagram and a strength of Strong, Worth exploring or Speculative, and the report ends with a top recommendation [improve-codebase-architecture § Process › 2. Present candidates as an HTML report]. Domain terms come from `GLOSSARY.md` and architecture terms from `codebase-design` [improve-codebase-architecture § Process › 2. Present candidates as an HTML report]. A candidate that contradicts an ADR is surfaced only when the friction justifies revisiting it, and is marked [improve-codebase-architecture § Process › 2. Present candidates as an HTML report]. It does not propose interfaces yet; it asks which candidate to explore [improve-codebase-architecture § Process › 2. Present candidates as an HTML report].

**Step three, grill.** Once a candidate is picked, the skill runs the `grilling` interview over constraints, dependencies, the shape of the deepened module, what sits behind the seam and what tests survive [improve-codebase-architecture § Process › 3. Grilling loop]. Side effects happen inline: naming a deepened module after a concept missing from `GLOSSARY.md` adds the term; sharpening a fuzzy term updates the glossary; a load-bearing rejection is offered as an ADR so future reviews do not re-suggest it, while ephemeral or self-evident reasons are skipped; and exploring alternative interfaces uses `codebase-design`'s design-it-twice pattern of parallel sub-agents [improve-codebase-architecture § Process › 3. Grilling loop] [codebase-design § Going deeper].

In use: in the de-slop video Matt runs it on his course-video-manager repository and it returns six candidates [how-to-de-slop-a-codebase-ruined-by-ai @ 00:06:14]. He picks one where front-end and back-end implementations of an insertion rule are separate and could drift apart, and the proposed fix gains locality by putting the rule in one module [how-to-de-slop-a-codebase-ruined-by-ai § Detailed source notes]. The conversation grounds the proposal in code, shapes a module interface, and could end as a GitHub issue for an unattended agent [how-to-de-slop-a-codebase-ruined-by-ai § Detailed source notes].

How often and with how much autonomy:

- It is not an away-from-keyboard skill. Matt says it demands judgement from the human, with the agent as the sergeant who finds candidates and the human as the general who decides what is good for long-term health [how-to-de-slop-a-codebase-ruined-by-ai § Detailed source notes].
- He recommends revisiting a fast-moving codebase regularly; in the video he says to run the skill every couple of days [how-to-de-slop-a-codebase-ruined-by-ai § Detailed source notes].
- On a legacy codebase, which he says in practice means a bad one with many shallow modules, the first need is a harness of tests around deep modules before changes are made, so running this skill is where to begin; the note records his point that seams and tests give a harness for safe changes [how-to-de-slop-a-codebase-ruined-by-ai § Detailed source notes].
- In the router it sits under "Codebase health" as upkeep for spare moments; a chosen candidate becomes an idea entering the main flow at `/grill-with-docs` [ask-matt § Codebase health].

## Domain modelling: glossary, ubiquitous language and ADRs

Matt's domain work is DDD-derived and, by his own account, new to him. In April 2026 he said he was "still on the edge of exploring" DDD but loved what he read [software-fundamentals-matter-more-than-ever @ 00:08:14]. The reason he adopted it was practical: the AI was too verbose, and he recognised the language gap that exists between a developer and a domain expert [software-fundamentals-matter-more-than-ever @ 00:07:41].

**Ubiquitous language.** In DDD, developer conversation, code and domain-expert conversation all derive from one domain model; Matt implements it as a markdown file of shared terms that he and the AI both use [software-fundamentals-matter-more-than-ever § Detailed source notes]. The first form was a `ubiquitous-language` skill that scans a codebase for terms and writes the glossary; he kept the file open while prompting and referenced it from the agent instructions file, wary of putting too much in that file [senior-dev-this-grill-me-prompt-is-going-viral-among-top-engineers § Detailed source notes] [senior-dev-this-grill-me-prompt-is-going-viral-among-top-engineers @ 00:09:46].

He argues the vocabulary suits AI because DDD's terms are already in the model's training [senior-dev-this-grill-me-prompt-is-going-viral-among-top-engineers § Detailed source notes]. The claimed benefits are less verbose reasoning, implementation closer to the plan, and code that is easier to search, which he says are observations, not measured outcomes [software-fundamentals-matter-more-than-ever § Detailed source notes] [i-stopped-using-grill-me-for-coding § Detailed source notes].

**From skill to workflow.** In "I stopped using Grill Me for coding" the problem was that inside a codebase the agent kept asking what local terms like "stand-alone video" meant, and the language found in a session was not saved [i-stopped-using-grill-me-for-coding § Detailed source notes]. The answer was `grill-with-docs`, which combines the grilling interview with a glossary file and decision records [i-stopped-using-grill-me-for-coding § Detailed source notes]. The current skill file is one line telling the agent to call `grilling` and `domain-modeling` [grill-with-docs § Source text]. His rule: with a codebase use grill-with-docs, without one use grill-me, even early in a project when shared language is being set [i-stopped-using-grill-me-for-coding @ 00:13:32]. The router restates this as stateful versus stateless [ask-matt § Standalone].

**Bounded contexts.** The glossary maps to DDD's bounded context; Matt admits "context" is overloaded [i-stopped-using-grill-me-for-coding @ 00:04:44]. A single-context repo has one glossary at the root; a multi-context repo has a `GLOSSARY-MAP.md` pointing at a glossary and an `adr` folder per context, plus system-wide ADRs at the root, and files are created lazily [domain-modeling § File structure].

**What the `domain-modeling` skill does.** It is the active discipline of building and sharpening the model, as against merely reading the glossary [domain-modeling § Source text]. During a session it:

- challenges the user when a term conflicts with the glossary;
- proposes a canonical term when language is vague or overloaded, for example asking whether "account" means the Customer or the User;
- stress-tests relationships with invented edge-case scenarios;
- cross-references the code and surfaces contradictions with what the user says;
- updates `GLOSSARY.md` inline as terms resolve, not in batches [domain-modeling § During the session].

The glossary must be devoid of implementation details and is not a spec or a scratch pad [domain-modeling § During the session]. In his live example the agent surfaces the one-to-many relationship between pitches and stand-alone videos, and Matt accepts some awkward wording because names can be refined later [i-stopped-using-grill-me-for-coding § Detailed source notes]. On the podcast his payoff example is naming a complicated ghost-lesson interaction the "materialization cascade", and he adds that the agent is good at proposing such terms [ai-skills-with-matt-pocock @ 00:57:10].

**ADRs.** The skill offers an ADR only when all three hold: the decision is hard to reverse, it would be surprising without context, and it is the result of a real trade-off between genuine alternatives [domain-modeling § During the session › Offer ADRs sparingly]. Matt's explanation is that a glossary does not capture non-obvious decisions, and he gives interchangeable library choices as the case that needs no ADR [i-stopped-using-grill-me-for-coding @ 00:07:46]. The ADR folder is also what keeps architecture reviews from re-litigating decisions [improve-codebase-architecture § Source text].

## What to document: delete most of the docs

Matt's rule on documentation follows from the same logic. He says the agent should be able to rely on the code itself, which puts a burden on code to be well formatted, sensibly laid out, and to separate interfaces from implementations [delete-most-of-your-docs § Detailed source notes]. His objection to a docs layer is that markdown in the repo is not executable or testable against the code, so it drifts and leaves the agent with two sources of truth [delete-most-of-your-docs § Detailed source notes]. This is his position, not a measured result [delete-most-of-your-docs § Detailed source notes].

He still wants three kinds of document that code cannot express: ADRs for rejected alternatives, a glossary, and a thin navigation layer [delete-most-of-your-docs § Detailed source notes]. The same reasoning drives his advice never to run Claude Code's init, since information findable in `package.json`, config, imports or file names goes stale when copied into a global file [never-run-claude-init § Detailed source notes]. He also deletes PRDs once implemented, because kept documents rot and mislead later agents [full-walkthrough-workflow-for-ai-coding-matt-pocock § Detailed source notes].

## Greenfield and brownfield

Asked whether his skills only work on greenfield code, Matt says there is no real greenfield; the difference is that you have not yet sorted out how to work in a brownfield codebase, which already has conventions and usually tests [there-is-no-such-thing-as-greenfield § Detailed source notes]. In greenfield you choose them up front, but once AI has written code the early choices harden, and a greenfield codebase stays green for a few days, a week at most [there-is-no-such-thing-as-greenfield § Detailed source notes].

## Views from guests, labelled as theirs

In the August 2026 stream with Robert C. Martin, Martin relates shaping a codebase so an agent can use a narrow interface to deep modules, noting that models also attend to tests and structural consistency [live-uncle-bob-software-fundamentals-ai § Detailed source notes]. Martin also describes a deterministic checker, which agents cannot violate, for allowed dependency directions [live-uncle-bob-software-fundamentals-ai @ 00:27:37]. These are Martin's practices, not Matt's.

## How Matt explains it

His teaching devices recur across videos, which makes them useful when an agent has to explain the ideas to a person:

- **The Memento guy.** The agent wakes up in the codebase with no memory, so the codebase has to be designed for a new starter. He extends this to "twenty new starters a day" [do-software-fundamentals-still-matter § Detailed source notes] [your-codebase-is-not-ready-for-ai @ 00:06:35].
- **Sergeant and general.** The agent is the tactical programmer on the ground; the human is the strategist above it [how-to-de-slop-a-codebase-ruined-by-ai § Detailed source notes] [software-fundamentals-matter-more-than-ever § Detailed source notes].
- **Blob diagrams.** He draws a codebase as a field of small squares for modules, shows the jumble an agent first sees, then regroups the squares into a few large blobs with thin interfaces on top [your-codebase-is-not-ready-for-ai § Detailed source notes].

## Changed over time

The dates are video publication dates from the corpus notes. For skill files the corpus records only the last-modified date of the source file, which is not a publication date.

- **Before 2026.** In 2022 Matt modelled a tweet composer as a statechart, listing user events first [modelling-statecharts-step-by-step § Detailed source notes]. In 2023-12-27 he said shared code inside an application, such as a utils folder, can be its most complex layer to type [application-vs-library-ts-typescript § Detailed source notes]. Neither uses the deep-module vocabulary.
- **2026-02-26.** "Your codebase is NOT ready for AI" is the earliest corpus note presenting deep modules, gray boxes and the file system matching the mental map [your-codebase-is-not-ready-for-ai § Detailed source notes]. Note this is only the earliest video in the corpus on it, not proof that he had not said it before.
- **2026-03-16 and 2026-03-18.** An architecture skill already exists, and its output was turned into a refactor RFC with `gh issue create` [five-claude-code-skills-i-use-every-day § Detailed source notes]. A feature build asks for a PRD with module design, tests and vertical slices [building-a-real-feature-with-claude-code § Detailed source notes].
- **2026-04-23.** At the AI Engineer conference he lays out the full argument from complexity and entropy to ubiquitous language to deep modules and gray boxes. He calls the DDD work something he is "still on the edge of exploring" [software-fundamentals-matter-more-than-ever § Detailed source notes] [software-fundamentals-matter-more-than-ever @ 00:08:14].
- **2026-04-29.** The architecture skill gained a glossary of terms (module, interface, depth, seam, adapter, locality, leverage) shortly before this video, and the video says its purpose is a common vocabulary for developer and model before refactor discussions [how-to-de-slop-a-codebase-ruined-by-ai § Detailed source notes] [how-to-de-slop-a-codebase-ruined-by-ai @ 00:01:14].
- **2026-05-07 to 2026-05-14.** The `ubiquitous-language` skill is described as scanning a codebase for terms [senior-dev-this-grill-me-prompt-is-going-viral-among-top-engineers § Detailed source notes]. A week later it is folded into grill-with-docs with a `context.md` and ADRs, so the glossary is built during the interview [i-stopped-using-grill-me-for-coding § Detailed source notes].
- **2026-08-15 to 2026-08-19.** Last-modified dates: `improve-codebase-architecture` and `domain-modeling` 2026-08-15, `codebase-design` and `to-spec` 2026-08-19 [improve-codebase-architecture § Original source frontmatter] [codebase-design § Original source frontmatter]. In this version the vocabulary is a separate `codebase-design` skill, whereas the April video places the glossary inside the architecture skill; the corpus does not date the split.
- **2026-09-17.** On the podcast he says he integrates DDD into every part of his setup, firmer than the April "still exploring", and calls his relationship with TDD mixed: agents have larger working memory than humans, so what they need is feedback loops [ai-skills-with-matt-pocock @ 00:57:10] [ai-skills-with-matt-pocock @ 01:18:18].
- **2026-09-26.** In the review talk he refers to a code-base-design skill for deepening opportunities and shared vocabulary, and moves coding standards out of the implementer into a separate review sub-agent [fixing-the-pr-bottleneck-matt-pocock-aihero § Detailed source notes].
- **2026-10-05.** The v1.3 changelog documents renaming CONTEXT.md to GLOSSARY.md [skills-changelog-v13 § Factual notes]. The retro discussion in the same release gives renaming a context document and consolidating glossary material as an example of a proposed change [new-skills-v1-3-pr-implement-spec-retro § Detailed source notes].
- **2026-10-06 and 2026-10-07.** Last-modified dates of the router, `tdd`, `diagnosing-bugs` and `code-review`. Seams are central: tests at pre-agreed seams, regression tests at correct seams, and codebase-design as a vocabulary other skills speak [ask-matt § Vocabulary underneath] [tdd § Seams: where tests go]. `tdd` also says refactoring belongs to review, not the red-green loop [tdd § Rules of the loop].
- **2026-10-13 and 2026-10-16.** Shorts restate the tactical-versus-strategic framing and "tell the agent to care", and a talk repeats that the code environment matters more than the spec [tell-your-agents-to-care-about-the-codebase § Detailed source notes] [your-agent-is-a-tactical-tornado-vo33ex § Detailed source notes] [matt-pocock-the-most-valuable-skill-in-the-age-of-ai § Detailed source notes].

Three patterns follow (synthesis from the entries above). The vocabulary hardened from a talk argument into a named skill with rejected framings. The domain-language work went from a standalone skill to an interview-integrated workflow to GLOSSARY.md. And testing advice moved toward few, agreed seams with refactoring pushed to a review pass, matching his description of implement-then-review as red-green-refactor across two context windows [fixing-the-pr-bottleneck-matt-pocock-aihero @ 00:13:32].

## Applying this

*Inference:* When asked to design or restructure a module in a repo, use the eight `codebase-design` terms and avoid "service", "component", "API" and "boundary", because the skill treats consistent vocabulary as its main contribution.

*Inference:* Before proposing a seam, check whether two adapters actually exist (for example production and an in-memory fake). If only one does, Matt's rule says the seam is hypothetical and should not be added.

*Inference:* When a candidate refactor looks like extracting a helper, run the deletion test first. Extracting pure functions for testability can fail the locality test if the bugs live in how they are called.

*Inference:* When fixing a bug, ask where a test could exercise the real call pattern. If no seam gives that, record the missing seam as the architectural finding instead of writing a unit test that gives false confidence.

*Inference:* Keep a short glossary of domain terms next to the code and update it the moment a term is resolved. Keep it free of implementation detail, and use its terms in names, tickets and commit messages so that searches work.

*Inference:* Reserve ADRs for decisions that are hard to reverse, surprising and a genuine trade-off. Swappable library choices do not qualify.

*Inference:* For a large legacy or AI-generated codebase, scope an architecture review to recent hot spots, let the user pick one candidate, and draft interfaces only after that, as the skill orders it.

*Inference:* Before trusting tests at the interface and skipping implementation review (the gray-box idea), confirm the module is not critical, such as money or security.

*Inference:* Matt's guidance is about code you can change. Applying the deep-module vocabulary to read-only or reverse-engineered code, for example to name recovered subsystems, is our extension, not his.

## Caveats

- **One period, one medium.** Nearly everything is from February to October 2026, in videos and skills about AI-assisted coding; the corpus has no long-form article or book by Matt on module design. The vocabulary is his packaging of other authors' ideas (Ousterhout, Feathers, Evans, Beck), so do not read it as their views.
- **Notes, not transcripts.** The YouTube notes are factual summaries, and `[slug @ time]` citations point to reviewed transcripts that are kept privately and not published here.
- **Skills change often.** Matt says he updates the skills regularly, so a video's description can differ from the pinned skill text [i-stopped-using-grill-me-for-coding @ 00:14:22].
- **Opinion, not measurement.** Claims about concise replies, fewer thinking tokens, easier navigation and agents' training incentives are his observations or readings, and the notes say so [i-stopped-using-grill-me-for-coding § Detailed source notes] [tell-your-agents-to-care-about-the-codebase § Detailed source notes].
- **Linked but not stored.** DEEPENING.md (dependency categories, seam discipline, replace-don't-layer testing) and DESIGN-IT-TWICE.md (parallel sub-agents designing the interface several ways) are only linked from the skill, so their contents are not described here [codebase-design § Going deeper]. The same applies to the HTML-REPORT, GLOSSARY-FORMAT and ADR-FORMAT files.
- **Neighbouring topics.** TDD technique, grilling, context management and the spec-to-tickets flow are separate topics, and TypeScript-level modelling (branded types, discriminated unions) is too.
- **Guest views are not Matt's.** Robert Martin's dependency checker, thresholds and TDD views are his own [live-uncle-bob-software-fundamentals-ai @ 00:33:50].
- **Status.** Sonnet draft, given a lead pass on 2026-10-24: citations checked mechanically and a sample of claims compared with corrected transcripts. The checker verifies headings and numbers only, not whether a non-numeric claim is faithful.
