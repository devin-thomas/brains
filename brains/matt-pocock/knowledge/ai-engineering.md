---
brain: matt-pocock
topic: ai-engineering
sources: [a-complete-guide-to-vercels-ai-sdk-the-essential-tool-for-shipping-ai-apps, agent-skills-with-matt-pocock, agents-are-dead-long-live-workflows, ai-agents-are-so-simple-to-build, ai-in-practice-from-cloud-privacy-risks-to-on-device-solutions-with-szymon-rybczak-matt-pocock, ask-matt, building-ai-apps-that-dont-suck-handoffs, building-great-agent-skills-the-missing-manual, building-llm-apps-that-dont-suck-guardrails, claude-code-tried-to-improve-init, claude-codes-system-tools-are-so-bloated, code-review, codebase-design, cursor-hardcore-review-skill, delete-most-of-your-docs, do-software-fundamentals-still-matter, do-you-even-need-human-review, domain-modeling, dont-underestimate-the-communication-barrier, dont-waste-time-on-specs-prototype-instead, fixing-the-pr-bottleneck-matt-pocock-aihero, force-claude-code-to-use-the-right-cli, framework-hell-tutorial-hell-now-skill-hell, frontend-is-harder-for-ai-than-backend, full-walkthrough-workflow-for-ai-coding-matt-pocock, get-started-with-the-model-context-protocol-2-minute-tutorial, grill-with-docs, grilling, handoff, handoff-is-my-new-favourite-skill, how-i-use-claude-code-for-real-engineering, how-llm-tools-work-under-the-hood, i-stopped-using-grill-me-for-coding, i-was-an-ai-skeptic-then-i-tried-plan-mode, im-thinking-about-changing-my-most-popular-skill, implement-spec, imposter-syndrome-is-lying-to-you-matt-pocock-the-developers-journey-episode-4, improve-codebase-architecture, kill-your-memorymd, learn-anything-with-the-teach-skill, live-ive-joined-the-ai-revolution, live-uncle-bob-software-fundamentals-ai, make-claude-code-give-you-answers-not-essays, matt-pocock-s-agentic-engineering-workflow-just-copy-him, matt-pocock-the-most-valuable-skill-in-the-age-of-ai, most-companies-are-not-ready-for-background-agents, most-devs-dont-know-how-llm-tokens-work, most-devs-dont-understand-how-llm-tokens-work, most-devs-dont-understand-what-agents-are, mutation-testing-is-awesome, my-agent-hallucinated-what-do-i-do, never-run-claude-init, never-trust-an-llm, new-skills-v1-1-wayfinder-research-implement, new-skills-v1-2-wait-what-writing-for-agents-grill-me, new-skills-v1-3-pr-implement-spec-retro, nine-things-wrong-with-grill-skills, open-sourced-afk-software-factory, prototype, red-green-refactor-is-op-with-claude-code, retro, ship-working-code-while-you-sleep-with-the-ralph-wiggum-technique, skills-changelog-v13, software-fundamentals-matter-more-than-ever, stop-giving-your-llm-more-tools, stop-running-your-checks-in-harness-hooks-zgy8_a, stop-stuffing-your-context-window-heres-why, tdd, tell-your-agents-to-care-about-the-codebase, tell-your-llm-to-sacrifice-grammar, the-hidden-security-risk-in-your-system-prompts, the-secret-to-building-robust-frontend-apps-with-agents, this-change-makes-grill-me-so-much-better, to-spec, to-tickets, total-typescript-tour-and-ama, triage, vibe-coders-wont-be-able-to-fix-bugs, wayfinder, wayfinder-nothing-is-too-big-to-plan-anymore, what-even-are-agents-and-workflows, what-is-the-dumb-zone, what-messages-you-send-to-the-llm-actually-look-like, when-should-you-not-use-grill-me, writing-for-agents, yes-llms-can-produce-more-than-just-text, your-agent-is-a-tactical-tornado-vo33ex, your-codebase-is-not-ready-for-ai, your-effort-level-is-too-damn-high]
drafted_by: sonnet
reviewed: 2026-10-24 by sonnet lead pass (citation check with tools/check_citations.py; claims spot-checked against corrected transcripts; not every claim re-read against its source)
---

# AI engineering (Matt Pocock on AI-assisted development and agents)

## Overview

This note covers what Matt Pocock says about building with and for LLMs: how models work, what agents are, how he runs coding agents day to day, and the skills, specs, tickets, tests and reviews around them. His TypeScript teaching is left to other knowledge files. The sources are his YouTube talks and Shorts (condensed review notes, not full transcripts), his AI Hero pages, and his MIT-licensed skills repository.

The view moves in stages. In 2023 he used ChatGPT in his streaming workflow but doubted it for real programming. [live-ive-joined-the-ai-revolution @ 00:00:00] [total-typescript-tour-and-ama @ 00:00:14] Through 2025 he taught LLM fundamentals, the AI SDK and agent design from the app developer's side. [a-complete-guide-to-vercels-ai-sdk-the-essential-tool-for-shipping-ai-apps @ 00:00:02] By his own account, agents became useful to him only around the end of 2025. [matt-pocock-the-most-valuable-skill-in-the-age-of-ai @ 00:00:01] From 2026 the channel is mostly a workflow of grilling, specs, tickets, unattended implementation, review and retrospective, packaged as small "skills". [ask-matt § The main flow: idea → ship]

Two threads run through all of it. One is that model limits (context, attention, no memory) are engineering constraints to design around. The other is that software fundamentals matter more with agents, because the codebase is the environment the agent works in. [do-software-fundamentals-still-matter @ 00:00:02]

## LLM fundamentals

### Tokens, context and messages

Matt teaches that an LLM does not read text as written: input is encoded into tokens, the model works on numbers, and output tokens are decoded back into text. [most-devs-dont-know-how-llm-tokens-work @ 00:00:01]

Cost follows from this, because input and output tokens are billed at different rates. [most-devs-dont-understand-how-llm-tokens-work @ 00:00:30] Each model has its own vocabulary, so one prompt can give different token counts on different models. [most-devs-dont-understand-how-llm-tokens-work @ 00:01:20] He says rarer words, under-represented languages and less common programming languages cost more tokens, so widely used languages are cheaper to send to a model. [most-devs-dont-understand-how-llm-tokens-work @ 00:07:30]

A conversation is a list of messages with user and assistant roles. [what-messages-you-send-to-the-llm-actually-look-like @ 00:00:02] A system prompt sits at the start of the history, and some models add reasoning tokens to the assistant message. [what-messages-you-send-to-the-llm-actually-look-like @ 00:00:21] [what-messages-you-send-to-the-llm-actually-look-like @ 00:00:40]

The context window holds all input and output tokens, and every model has a fixed maximum. [stop-stuffing-your-context-window-heres-why @ 00:00:10] 

He warns of the "lost in the middle" effect: the start and end of the history influence output most, and a large window can still retrieve poorly. [stop-stuffing-your-context-window-heres-why @ 00:01:20] His takeaway is that fewer tokens in context give better results, even where the window is large. [stop-stuffing-your-context-window-heres-why @ 00:01:44]

### Tools

His explanation of tools is deliberately plain. A tool has a name, a description and typed parameters in JSON Schema, and the definition is injected into the system prompt as text. [how-llm-tools-work-under-the-hood @ 00:00:12] A tool call from the model is only an instruction; nothing has happened on the machine yet. [how-llm-tools-work-under-the-hood @ 00:00:55]

The developer's code executes it and returns the result with the same ID, and errors must be shown to the model so it can try something else. [how-llm-tools-work-under-the-hood @ 00:01:30] His summary is that tools are a way of getting the model to produce messages that the developer intercepts, executes and answers. [how-llm-tools-work-under-the-hood @ 00:02:04]

From that he derives a rule: do not give the model more tools than the task needs. Tool definitions work for roughly six to eight tools depending on the model, and with even around 12 the model can struggle to choose. [stop-giving-your-llm-more-tools @ 00:00:00] [stop-giving-your-llm-more-tools @ 00:00:24] MCP servers make it easy to pile on tools, so he advises keeping only those a task needs. [stop-giving-your-llm-more-tools @ 00:00:47]

### Structured output, prompting and cost levers

With the AI SDK he swaps free text for a typed object: `generateObject` takes a Zod schema, the schema is converted to JSON Schema, and Zod validates the result at runtime. [yes-llms-can-produce-more-than-just-text @ 00:00:31] [yes-llms-can-produce-more-than-just-text @ 00:00:52]

One cheap trick is to tell the model to be extremely concise and sacrifice grammar. He says this gives readable shorthand notes, and shorter output is also cheaper. [tell-your-llm-to-sacrifice-grammar @ 00:00:00] He keeps the instruction in his global CLAUDE.md. [make-claude-code-give-you-answers-not-essays @ 00:00:17]

### Hallucination and injection

Matt treats hallucination as expected. He describes model weights as a compression of a far larger training set, which keeps patterns but loses specifics. [never-trust-an-llm @ 00:05:25]

His diagnosis for a coding hallucination starts with one question: was the information in the context window? If not, it is a factuality hallucination, and the fix is to load the information. [my-agent-hallucinated-what-do-i-do @ 00:00:03] [my-agent-hallucinated-what-do-i-do @ 00:00:16] If it was there and the agent ignored it, it is a faithfulness hallucination, which he partly blames on attention degradation, and the remedy is to leave the dumb zone. [my-agent-hallucinated-what-do-i-do @ 00:00:43] [my-agent-hallucinated-what-do-i-do @ 00:01:20]

On prompt injection, his demo shows scraped data in a system prompt taking over the reply. Moving the data into user messages did not fix it. Wrapping it in XML tags that mark both start and end did, and he recommends putting retrieved data in user messages inside such tags. [the-hidden-security-risk-in-your-system-prompts @ 00:00:40] [the-hidden-security-risk-in-your-system-prompts @ 00:01:01] [the-hidden-security-risk-in-your-system-prompts @ 00:01:22] [the-hidden-security-risk-in-your-system-prompts @ 00:01:51]

## Agents, workflows and AI apps

### Definitions

Matt's definition came in September 2025 and he has held to it. A workflow is a chain of LLM calls on predetermined steps written in code, and an agent is an LLM given tools that chooses which to call and decides when it is finished. [what-even-are-agents-and-workflows @ 00:00:10] [what-even-are-agents-and-workflows @ 00:00:24] The crucial question is who decides when to stop. [what-even-are-agents-and-workflows @ 00:00:46]

He says agents suit tasks whose steps are unclear, while workflows suit repeated tasks with clear specifications and often do better. [what-even-are-agents-and-workflows @ 00:01:06] He adds that he is more excited by workflows, while allowing that he may be old-fashioned. [what-even-are-agents-and-workflows @ 00:01:44]

An earlier Short (June 2025) drew the same line: a workflow is a deterministic sequence with AI as a bounded part, and an agent hands control flow to the model. [agents-are-dead-long-live-workflows @ 00:00:35] A workflow lets the app ask clarifying questions, stop after a set number of iterations and evaluate the answer. [agents-are-dead-long-live-workflows @ 00:01:00]

In October 2025 he answered OpenAI's AgentKit announcement by calling the shown flow a workflow, not an agent, and treating the two as a spectrum with the test "can the LLM break the loop". [most-devs-dont-understand-what-agents-are § Detailed source notes] He hopes Anthropic's definition, tool calls in a loop, becomes the shared meaning. [most-devs-dont-understand-what-agents-are § Detailed source notes]

### An agent is a loop

His demonstration is minimal. The state is a message history and a step counter; each step is an LLM call, and the finish reason tells the loop to stop. [ai-agents-are-so-simple-to-build @ 00:00:02] [ai-agents-are-so-simple-to-build @ 00:01:01] Frameworks usually hide this loop, but the mechanism is the same. [ai-agents-are-so-simple-to-build @ 00:02:11]

### Designing AI apps

Matt's app-level advice stresses structure over model quality. He says the gap between strong and weak AI apps is more about user experience than the model. [agents-are-dead-long-live-workflows @ 00:00:00]

The handoff pattern puts a receptionist LLM in front of specialist LLMs, so each can be tuned separately and a change to course creation cannot disturb billing. [building-ai-apps-that-dont-suck-handoffs @ 00:00:14] [building-ai-apps-that-dont-suck-handoffs @ 00:00:43] Guardrails sit between user and model on input and output, often as a call to a smaller, cheaper model, and he says building your own is more predictable and cheaper. [building-llm-apps-that-dont-suck-guardrails § Detailed source notes]

### MCP

In March 2025 he showed the smallest MCP server: a TypeScript file with one tool described by a Zod schema, a standard IO transport, and registration with Claude Code. [get-started-with-the-model-context-protocol-2-minute-tutorial @ 00:00:12] His pitch was that any local task, from Docker setup to database migrations, can be exposed as a tool. [get-started-with-the-model-context-protocol-2-minute-tutorial @ 00:02:00]

His own caution grew. He later described MCP as uncertain for now, noted that Playwright MCP is context-heavy because of screenshots and tool definitions, and in June 2026 advised, as a first step, deleting every MCP server along with skills, plugins and instruction files, then watching what the agent does and layering procedures back. [imposter-syndrome-is-lying-to-you-matt-pocock-the-developers-journey-episode-4 @ 00:21:21] [frontend-is-harder-for-ai-than-backend @ 00:03:53] [matt-pocock-s-agentic-engineering-workflow-just-copy-him @ 00:56:00]

## Working with coding agents

### Context hygiene: smart zone and dumb zone

Matt attributes the smart-zone and dumb-zone framing to Dex Horthy. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:03:00] Output quality drops as the context fills, and each new token must relate to every other, so relationships grow faster than tokens. [what-is-the-dumb-zone @ 00:00:06] Performance sinks gradually rather than at one cliff. [what-is-the-dumb-zone @ 00:00:31]

The threshold moved as he re-estimated it (see "Changed over time"). His working answer is to break work into small chunks that run in fresh windows. [what-is-the-dumb-zone @ 00:01:12] A larger advertised window does not change this: he says a one-million-token window mainly helps retrieval. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:36:00]

His options at the edge are to clear, compact, hand off or redesign the task. [what-is-the-dumb-zone @ 00:01:12] He dislikes compacting. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:07:30] A fresh-context reviewer is stronger than a reviewer deep in the dumb zone. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 01:03:00]

The ask-matt router orders the choice at a phase boundary: continue, clear, hand off, subagent, or compact. [ask-matt § Phase boundaries] It reserves handoff for a new harness, a new directory, a colleague, or forking a side task. [ask-matt § Phase boundaries]

The handoff skill writes a summary to the OS temp directory, points to other artifacts instead of copying them, names suggested skills, and redacts keys and personal data. [handoff § Source text] In use it lets one out-of-scope task move to its own session while the original stays focused. [handoff-is-my-new-favourite-skill @ 00:00:00]

### Harness, instructions and memory

Matt says people focus too much on the model and not enough on the harness: prompts, skills, the environment, and the codebase. [matt-pocock-s-agentic-engineering-workflow-just-copy-him @ 00:00:00] He says his trimmed Claude Code setup cut the starting prompt and tool definitions from about 25K to about 8K tokens. [claude-codes-system-tools-are-so-bloated @ 00:01:02]

On instruction files he is blunt. He says never run `/init`: oversized or stale guidance consumes tokens, and most of it can be discovered from the repo. [never-run-claude-init @ 00:00:00] [never-run-claude-init @ 00:03:08] A global CLAUDE.md cannot deterministically stop a command, so for enforcement he asks for a pre-tool-use hook, and for style he suggests ESLint rules. [force-claude-code-to-use-the-right-cli @ 00:02:09] [force-claude-code-to-use-the-right-cli @ 00:04:14]

When Claude Code improved `/init` in March 2026, he praised the smaller output but still preferred a compact instruction file and a skill for rarely needed workflows. [claude-code-tried-to-improve-init @ 00:05:05]

His rule for where guidance lives is push versus pull: standards that always apply go in CLAUDE.md, while skills are pulled by the agent when needed. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 01:24:00] The retro skill adds that these files should be used very sparingly, usually only for navigation pointers. [retro § Reference › Files]

On documentation, he says teams should delete most docs that restate code, because they drift and give the agent two sources of truth. [delete-most-of-your-docs @ 00:00:45] What he keeps: decision records for rejected alternatives, a glossary of domain language, and a thin navigation layer. [delete-most-of-your-docs @ 00:01:45]

He turns agent auto-memory off. A stateless agent behaves more predictably, and persistent preferences belong in CLAUDE.md or a repo doc that the user edits. [kill-your-memorymd @ 00:00:43] [kill-your-memorymd @ 00:01:07]

He also says not to run checks in per-edit harness hooks: they add latency and noise, so batch a fast subset at pre-commit and leave heavy validation to CI. [stop-running-your-checks-in-harness-hooks-zgy8_a § Detailed source notes]

### Effort, models and tooling

Matt says almost everyone gets better value from a lower effort level, because higher effort spends more thinking tokens and pushes harder on things that do not need it. [your-effort-level-is-too-damn-high @ 00:00:02] [your-effort-level-is-too-damn-high @ 00:00:09] Benchmark charts hold tasks static, but a developer can specify tasks better and finish at lower effort. [your-effort-level-is-too-damn-high @ 00:00:30] He recommends starting at the lowest level and raising it only when needed. [your-effort-level-is-too-damn-high @ 00:00:50]

His own setup is Claude Code with Opus at medium effort for planning and local implementation. [matt-pocock-s-agentic-engineering-workflow-just-copy-him @ 00:24:00]

He argues grilling benefits from a strong model, because it draws on broad parametric knowledge, while implementation can use a weaker model when the plan and code give enough context. [nine-things-wrong-with-grill-skills @ 00:08:40] In his Sandcastle setup he uses Sonnet for implementation and Opus for review. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 01:27:00]

## The agentic workflow, step by step

### Early shapes: plan mode, phases and issues

In October 2025 his "real engineering" demo explored the repository, answered the agent's questions before any code, then asked for a multi-phase plan. [how-i-use-claude-code-for-real-engineering @ 00:00:00] He moved the plan into a GitHub issue so a fresh context could implement one phase. [how-i-use-claude-code-for-real-engineering @ 00:07:39]

In January 2026 he described plan mode as a read-only discovery pass, and said he had gone from skeptic to user. [i-was-an-ai-skeptic-then-i-tried-plan-mode @ 00:00:00] His CLAUDE.md asked for extremely concise plans that list unresolved questions. [i-was-an-ai-skeptic-then-i-tried-plan-mode @ 00:07:39]

By April 2026 he called multi-phase plans a practice he used until late 2025, and preferred a "grill me" session over default plan mode. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:05:00] [software-fundamentals-matter-more-than-ever @ 00:03:40]

### The Ralph loop

His script is bounded by a maximum iteration count and a completion marker, and feeds each run a JSON requirements document and a progress log. [ship-working-code-while-you-sleep-with-the-ralph-wiggum-technique @ 00:04:50] Feedback comes from type checks, unit tests, green CI and browser checks. [ship-working-code-while-you-sleep-with-the-ralph-wiggum-technique @ 00:10:19]

By June 2026 Matt said a loop is only one pattern. He prefers an away-from-keyboard agent that takes one scoped task from a queue, such as GitHub issues labelled for implementation. [matt-pocock-s-agentic-engineering-workflow-just-copy-him @ 00:42:00]

### The April 2026 workshop flow

His fullest account is the workshop recorded in April 2026. He splits work into human-in-the-loop tasks (planning and alignment) and AFK tasks (implementation). [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:26:00] He calls the human-reviewed planning stage the day shift and the agent-run implementation stage the night shift. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:46:00]

The flow: a grilling session to align, a PRD and Kanban board that humans review, agent implementation, then QA and code review, with QA feeding new issues back into the board. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 01:33:00]

### Sandboxes and parallel agents

He built Sandcastle, a TypeScript library that runs a prompt, an agent and a sandbox together, because unattended agents with permission checks off need isolation. [open-sourced-afk-software-factory @ 00:00:00] The template he shows has a planner that picks unblocked issues, an implementer in its own sandbox and branch per issue, a reviewer and a merger. [open-sourced-afk-software-factory @ 00:05:14] He stresses this arrangement is a template, not a required architecture. [open-sourced-afk-software-factory @ 00:05:14]

### The ask-matt main flow

The skills repository encodes the flow. Start with `/grill-with-docs`; detour through `/prototype` if a question needs runnable code; for multi-session work use `/to-spec` then `/to-tickets`; build with `/implement` per ticket or `/implement-spec` for the whole spec; finish with `/code-review`, and close the loop with `/retro`. [ask-matt § The main flow: idea → ship]

It advises keeping the grilling, spec and tickets in one unbroken context window, and each `/implement` fresh. [ask-matt § The main flow: idea → ship › Context hygiene] For bug reports and requests you did not create, `/triage` moves issues through category and state roles toward `ready-for-agent`. [ask-matt § On-ramps] [triage § Roles]

### Where humans stay

Matt does not want every checkpoint automated at once. For large work, align early with the AI and still review before production. [do-you-even-need-human-review @ 00:00:26] For small changes, skip alignment and review the result. [do-you-even-need-human-review @ 00:00:45]

The rule is to shift checkpoints left for big work that needs alignment, and right for small work whose diff is easy to regenerate. [do-you-even-need-human-review @ 00:01:41]

Manual QA is where taste is imposed, and he says fully automated creation, QA and research produce slop. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 01:11:00]

## Grilling and shared language

### The grilling skill

`/grill-me` is Matt's most used skill, a short instruction that makes the agent interview the user until both share an understanding. [im-thinking-about-changing-my-most-popular-skill @ 00:00:02] [agent-skills-with-matt-pocock @ 00:05:00] His theory is that hiding the next step from the agent keeps it in the interview stage rather than rushing to a plan. [agent-skills-with-matt-pocock @ 00:05:00]

He frames the goal through Brooks's "design concept": a shared, unwritten theory of what is being built, reached before planning. [software-fundamentals-matter-more-than-ever @ 00:03:40] He does not usually read the PRD closely afterwards, since the grilling already created the alignment. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:31:00]

The current skill text maps the conversation as a design tree and asks the whole frontier of ready questions in a round, each with a recommended answer worded so that "yes" accepts it. [grilling § Source text] Finding facts is the agent's job, so it dispatches a sub-agent to look them up; the decisions stay with the user. [grilling § Source text] It ends when the frontier is empty and the user confirms. [grilling § Source text]

### Grill with Docs and domain language

In May 2026 he stopped using plain `/grill-me` for coding. The agent kept asking what local terms meant, and shared language found in a session was not saved. [i-stopped-using-grill-me-for-coding @ 00:00:00] His replacement combines grilling with a glossary based on domain-driven design's ubiquitous language. [i-stopped-using-grill-me-for-coding @ 00:03:40]

`grill-with-docs` simply calls the grilling and domain-modeling skills together. [grill-with-docs § Source text] Domain modeling challenges the user's terms against the glossary, sharpens fuzzy words, tests concrete scenarios, cross-checks the code, and updates the glossary inline. [domain-modeling § During the session › Challenge against the glossary] [domain-modeling § During the session › Cross-reference with code] It offers an ADR only if the decision is hard to reverse, surprising, and a real trade-off. [domain-modeling § During the session › Offer ADRs sparingly]

### Using grilling well

His "9 things" video says to separate questions answerable in conversation from high-fidelity questions about feel, which should go to a prototype through a handoff. [nine-things-wrong-with-grill-skills @ 00:00:00] He warns that a broad scope hides those questions and pushes the session into the dumb zone, and to be an active participant rather than a passive answerer. [nine-things-wrong-with-grill-skills @ 00:03:43] [nine-things-wrong-with-grill-skills @ 00:05:41]

He alternates between two active grilling sessions, sometimes three, to raise throughput. [nine-things-wrong-with-grill-skills @ 00:08:40]

His decision rule: for a small diff, use a one-shot prompt; for larger work, grill; if the interview exposes something too large for one planning session, use Wayfinder. [when-should-you-not-use-grill-me § Detailed source notes]

## From specs to tickets

### Specs are destinations, not source

Matt is skeptical of spec-driven development. In his experiments, a workflow that edits the spec and re-runs the compiler without reading code produced progressively worse code. [software-fundamentals-matter-more-than-ever @ 00:00:20] He calls writing specs and discarding code vibe coding by another name. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:13:00]

He treats a spec as a short-lived artifact that schedules work across sessions, and says he does not favour persisting specs. [agent-skills-with-matt-pocock @ 00:17:30] He usually deletes or closes PRDs once implemented because kept documents rot and mislead later agents. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 01:24:00]

In July 2026 he renamed PRD and issue skills to "spec" and "tickets" because the old terms were too narrow. [new-skills-v1-1-wayfinder-research-implement @ 00:00:37]

The `/to-spec` skill does not interview: it synthesizes what the conversation and codebase already show. [to-spec § Source text] It sketches test seams first and prefers the highest seam, with one seam ideal. [to-spec § Process] The template has a problem statement, solution, a long list of user stories, implementation decisions, testing decisions and out-of-scope notes. [to-spec § Problem Statement] [to-spec § User Stories] It says not to include file paths or code snippets, which go stale. [to-spec § Implementation Decisions]

### Tracer-bullet tickets

`/to-tickets` breaks the work into tracer-bullet vertical slices. Each slice cuts a narrow but complete path through every layer and is sized to fit in a single fresh context window. [to-tickets § Process › 3. Draft vertical slices] Each ticket declares blocking edges, so a ticket whose blockers are done is on the frontier. [to-tickets § Process › 3. Draft vertical slices]

Matt argues AI tends to code horizontally, layer by layer, which delays feedback, so he asks for vertical slices. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:40:00]

Before publishing, the skill quizzes the user on granularity and blocking edges until they approve. [to-tickets § Process › 4. Quiz the user] Tickets go to a configured tracker, or to one file each under a local scratch folder when the tracker is local files. [to-tickets § Process › 5. Publish the tickets to the configured tracker]

### Wayfinder for foggy work

Wayfinder is for a large idea with unresolved questions that cannot fit one agent session. [new-skills-v1-1-wayfinder-research-implement @ 00:07:53] It charts a shared map on the issue tracker, then works decision tickets one at a time, producing decisions, not deliverables. [wayfinder § Source text]

The map is an index, not a store. [wayfinder § The Map] Tickets are typed research, prototype, grilling or task, and are sized to one 100K-token agent session. [wayfinder § Ticket Types] [wayfinder § Out of scope › Tickets] Out-of-view decisions sit as "fog of war" in a Not yet specified section. [wayfinder § Fog of war] A session never resolves more than one ticket, except research. [wayfinder § Invocation]

When the map clears it hands off at `/to-spec` rather than building. [ask-matt § On-ramps] He says a graph beats a single spec when the work has substantial fog or needs several sessions, and that simpler work can go the direct route. [wayfinder-nothing-is-too-big-to-plan-anymore @ 00:08:40]

### Prototype and research

Matt's advice is to prototype instead of writing long specs when a design or logic question needs a runnable answer. [dont-waste-time-on-specs-prototype-instead @ 00:00:00] The skill defines a prototype as throwaway code that answers a question, either a single HTML logic demo or several UI variants on one route. [prototype § Source text] [prototype § Pick a branch]

The prototype is kept as a primary source on a throwaway branch, with a pointer from the implementation issue, while main keeps only the validated decision. [prototype § Rules that apply to both]

## Testing with agents

### Red, green, one slice at a time

In his February 2026 video, Matt presented red-green-refactor as especially valuable with agents. Watching a test fail and then pass makes the agent's work legible. [red-green-refactor-is-op-with-claude-code @ 00:02:00] He still does manual QA afterwards, since tests can be weak. [red-green-refactor-is-op-with-claude-code @ 00:02:00]

He warns against a big batch of tests followed by a one-shot implementation. [red-green-refactor-is-op-with-claude-code @ 00:03:01] In the April workshop he said AI tends to write weak tests unless the test comes first. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 01:03:00]

The tdd skill states the same rules: red before green, one slice at a time, test only at pre-agreed seams, and verify behaviour through public interfaces. [tdd § Rules of the loop] [tdd § Seams: where tests go] [tdd § What a good test is] Its anti-patterns are implementation-coupled tests, tautological tests, and horizontal slicing. [tdd § Anti-patterns]

In July 2026 he moved refactoring out of the TDD loop and into code review, so the implementation stays focused. [new-skills-v1-1-wayfinder-research-implement @ 00:11:50]

### Feedback loops and test quality

He says the quality of an agent's output is capped by the quality of its feedback loops. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 01:08:00] Types, tests and a fast CI are the constraints that keep an agent on course. [most-companies-are-not-ready-for-background-agents @ 00:01:03]

He worries that checks can lie. Examples he gives include tests that restate the implementation, tests of source order, and tests that cannot fail because an API was stubbed out. [fixing-the-pr-bottleneck-matt-pocock-aihero @ 00:03:00]

His answer for suite quality is mutation testing: a tool edits the source and reruns the suite, and surviving mutants give the agent rich feedback to write stronger tests. [mutation-testing-is-awesome @ 00:00:16] [mutation-testing-is-awesome @ 00:00:31] Martin, in conversation with Matt, uses a "verification gauntlet" of coverage, static checks and mutation testing. [live-uncle-bob-software-fundamentals-ai @ 00:00:00]

For the frontend, he says agents get weaker feedback because layout and interaction are visual, and a browser tool can supply it at a context cost. [frontend-is-harder-for-ai-than-backend @ 00:00:00] [frontend-is-harder-for-ai-than-backend @ 00:03:53] He adds that reducers separate UI from testable state logic and make agent-edited pages sturdier. [the-secret-to-building-robust-frontend-apps-with-agents § Detailed source notes]

## Code review and the PR bottleneck

### Two axes, separate reviewers

The code-review skill checks the diff against a fixed point on two axes, Standards and Spec, run as parallel sub-agents so they do not pollute each other's context. [code-review § Source text] Standards always carries a baseline of Fowler code smells, and the repo's documented standards override it. [code-review § Process › 3. Identify the standards sources] The skill does not merge or rerank the axes. [code-review § Process › 5. Aggregate]

Its reasoning is that a change can pass one axis and fail the other. [code-review § Why two axes]

### The September 2026 talk

In "Fixing the PR Bottleneck", he describes a software factory where agents start work and deterministic triggers feed it. Without brakes it produces pull requests nobody can review. [fixing-the-pr-bottleneck-matt-pocock-aihero @ 00:00:02] The brakes are automated checks, automated agent review, and human review. [fixing-the-pr-bottleneck-matt-pocock-aihero @ 00:02:00]

He says standards placed in the implementer's instructions degrade its output, because implementation is already heavy. Review should be a separate skill run in a sub-agent that reads a team-owned standards file. [fixing-the-pr-bottleneck-matt-pocock-aihero @ 00:11:30] The retro skill gives the reason: implementation has the most context pressure and review has the least, so review should impose the standards. [retro § Reference › Implementation vs Review]

When he tried Cursor's ambitious review prompt, he accepted some structural findings, rejected others, and found a false positive from missing system context. [cursor-hardcore-review-skill @ 00:08:40]

A `/pr` skill classifies changes as one-way or two-way doors and adds merge-danger and blast-radius summaries. [fixing-the-pr-bottleneck-matt-pocock-aihero @ 00:16:00] The v1.3 changelog frames the problem with the line "Writing code got cheap. Reading it did not." [skills-changelog-v13 § Short source quotation]

### Retro and compounding

`/retro` looks back over a session and suggests changes to the agent's environment, not the code. [ask-matt § The main flow: idea → ship] Its categories are navigation, automated checks, coding standards, bloated global instructions, tool economy, no-ops and information access. [retro § Steps]

Mechanical mistakes get a deterministic check, and `CODING_STANDARDS.md` is reserved for judgement calls. [retro § Steps] The suggestions are presented for human approval, not self-applied. [new-skills-v1-3-pr-implement-spec-retro @ 00:10:30]

### Implement-spec

`/implement-spec` treats tickets as a task graph and runs implementer subagents across the ready frontier, each in its own worktree and driving `/tdd`. [implement-spec § Source text] [implement-spec § Steps] A merger subagent merges each result into an integration branch, and one code review runs at the end. [implement-spec § Steps]

He presents it as a middle ground between a manual loop and a deterministic script, which he says is more reliable and cheaper. [new-skills-v1-3-pr-implement-spec-retro @ 00:00:28]

## Writing for agents and skills

### Skills as procedures

In June 2026 he described "skill hell": many freely available skills with no way to judge them. [building-great-agent-skills-the-missing-manual @ 00:00:01] His checklist is the trigger, internal structure, steering and pruning. [building-great-agent-skills-the-missing-manual @ 00:02:20]

His own set leans user-invoked, because model-invoked skills add descriptions to every request and the agent may not follow the pointer. [building-great-agent-skills-the-missing-manual @ 00:03:00] He separates procedures the human runs from abilities the model invokes. [matt-pocock-s-agentic-engineering-workflow-just-copy-him @ 00:17:00]

He says skills should empower the user, and that piling many skill frameworks into one agent overloads the model. [framework-hell-tutorial-hell-now-skill-hell § Detailed source notes]

### The writing-for-agents rules

The skill treats a skill description or an AGENTS.md line as a context pointer, whose wording decides when the agent reaches the material. [writing-for-agents § Context pointers] It counts two loads: context load on the agent's window and cognitive load on the human. [writing-for-agents § The two loads]

Content sits on an information hierarchy of in-file steps, in-file reference, and disclosed reference behind pointers. [writing-for-agents § Information hierarchy] Every step ends on a clear completion criterion, because vague bounds invite premature completion. [writing-for-agents § Steps and completion criteria]

Leading words are compact concepts already in the model's pretraining, such as "tracer bullets", repeated as a token to anchor behaviour. [writing-for-agents § Leading words] The same section warns that negation drags the forbidden behaviour into context, so prompt the positive. [writing-for-agents § Leading words]

For pruning it asks for a single source of truth, treats the environment as a source of truth, and hunts sediment and no-ops. [writing-for-agents § Pruning] In v1.2 he added a plain-English `/wait-what` and drew on a simplified technical English standard for the writing skill. [new-skills-v1-2-wait-what-writing-for-agents-grill-me @ 00:02:40] [new-skills-v1-2-wait-what-writing-for-agents-grill-me @ 00:04:00]

## Codebase design for agents

### Strategic versus tactical

Matt borrows Ousterhout's tactical-versus-strategic split. He says agents are good tactical programmers and weak at strategy, partly because they are rewarded on tactical benchmarks. [tell-your-agents-to-care-about-the-codebase @ 00:00:00] The developer must be the annoying colleague who asks for the refactor first, without reading every line. [tell-your-agents-to-care-about-the-codebase @ 00:00:45]

When told the codebase should improve over time, he says, agents care and do strategic work well. [tell-your-agents-to-care-about-the-codebase @ 00:01:45] He frames his agent as a tactical tornado unless steered. [your-agent-is-a-tactical-tornado-vo33ex § Detailed source notes]

He compares an agent to the amnesiac in Memento: each session starts by re-learning the codebase, so the codebase's explorability matters more than model strength. [do-software-fundamentals-still-matter @ 00:01:10]

### Deep modules and gray boxes

He recommends deep modules, with small interfaces hiding substantial behaviour, as easier for AI to navigate and test. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 01:14:00] The advice is to design and verify the interface yourself and delegate the implementation behind it, a "gray box". [your-codebase-is-not-ready-for-ai @ 00:03:03]

The codebase-design skill defines module, interface, depth, seam, adapter, leverage and locality. [codebase-design § Glossary] `/improve-codebase-architecture` surfaces deepening candidates as an HTML report, then grills the chosen one. [improve-codebase-architecture § Process › 2. Present candidates as an HTML report] [improve-codebase-architecture § Process › 3. Grilling loop]

## Teaching, learning and careers

His teach skill is stateful: it keeps a mission, a learning record, a glossary and lessons so each session adapts to the learner. [learn-anything-with-the-teach-skill @ 00:00:00] [learn-anything-with-the-teach-skill @ 00:02:40]

## How Matt explains it

- **Mechanism first, in the smallest code.** He shows the tool loop and the agent loop in a few lines and says the whole agent is a loop with a step counter and a history. [ai-agents-are-so-simple-to-build @ 00:01:41] Tools are plain text in the prompt. [how-llm-tools-work-under-the-hood @ 00:00:12]
- **Analogies.** The model weights are lossy compression. [never-trust-an-llm @ 00:05:25] The agent is Memento's amnesiac. [do-software-fundamentals-still-matter @ 00:01:10] Handoffs are a receptionist. [building-ai-apps-that-dont-suck-handoffs @ 00:00:43] The agent is a contractor, so the barrier is real either way. [dont-underestimate-the-communication-barrier § Detailed source notes]

## Changed over time

- **Skeptic to user.** In March 2023 he said ChatGPT is less useful than it seems for real programming. [total-typescript-tour-and-ama @ 00:00:14] In April 2023 he started using ChatGPT in his streaming workflow. [live-ive-joined-the-ai-revolution @ 00:00:00] In 2025 he recalled first reading AI as another hype cycle, then changing his mind after colleagues urged him to look closer. [imposter-syndrome-is-lying-to-you-matt-pocock-the-developers-journey-episode-4 @ 00:24:56]
- **Inline completion to agents.** In October 2025 he mostly relied on inline Copilot completion and rarely used agentic sidebars. [ai-in-practice-from-cloud-privacy-risks-to-on-device-solutions-with-szymon-rybczak-matt-pocock @ 00:07:09] By October 2026 he says agents became useful for him around the end of 2025. [matt-pocock-the-most-valuable-skill-in-the-age-of-ai @ 00:00:01]
- **Vibe coding.** In May 2025 he said code is cheap, so deleting a feature and rebuilding it may beat debugging it. [vibe-coders-wont-be-able-to-fix-bugs @ 00:00:27] By April 2026 he called discarding code behind specs a mistake and said the code is the environment. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:13:00]
- **Agents versus workflows.** In June 2025 the line was "agents are dead, long live workflows". [agents-are-dead-long-live-workflows @ 00:00:35] In September 2025 he formalised the definitions. [what-even-are-agents-and-workflows @ 00:00:10] By 2026 he was running unattended loops himself, but with a queue of scoped tasks. [matt-pocock-s-agentic-engineering-workflow-just-copy-him @ 00:42:00]
- **Plan, phases, Ralph, Wayfinder.** Multi-phase plans (until late 2025) gave way in January 2026 to the Ralph task loop, then to grilling plus specs and tickets, and in July 2026 to Wayfinder for foggy work. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:05:00] [ship-working-code-while-you-sleep-with-the-ralph-wiggum-technique @ 00:00:00] [new-skills-v1-1-wayfinder-research-implement @ 00:07:53]
- **Smart-zone threshold.** About 100K tokens in April 2026. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 00:03:00] About 120K in May. [nine-things-wrong-with-grill-skills @ 00:03:43] About 150K in July, by his own account up from 100K six months earlier. [what-is-the-dumb-zone @ 00:00:53]
- **Grill-me cadence.** Version 1.1 (July 2026) made it ask one question at a time and wait for confirmation. [new-skills-v1-1-wayfinder-research-implement @ 00:00:37] On 15 July he said he had a local copy asking everything at once. [im-thinking-about-changing-my-most-popular-skill @ 00:00:15] Two days later he announced rounds of dependent questions. [this-change-makes-grill-me-so-much-better @ 00:01:01] The current skill asks the whole ready frontier per round. [grilling § Source text]
- **PRD to spec.** The PRD and issue names became spec and tickets in July 2026. [new-skills-v1-1-wayfinder-research-implement @ 00:00:37] The v1.3 changelog also renamed CONTEXT.md to GLOSSARY.md. [skills-changelog-v13 § Factual notes]
- **Instruction files.** In April 2026 he put always-applying standards in CLAUDE.md. [full-walkthrough-workflow-for-ai-coding-matt-pocock @ 01:24:00] In June 2026 he advised deleting CLAUDE.md and AGENTS.md as a reset, then layering back only what proved needed. [matt-pocock-s-agentic-engineering-workflow-just-copy-him @ 00:56:00]
- **Checks.** In February 2026 he showed a hook to enforce a CLI rule. [force-claude-code-to-use-the-right-cli @ 00:02:09] In October 2026 he said not to run checks in per-edit hooks. [stop-running-your-checks-in-harness-hooks-zgy8_a § Detailed source notes]
- **Effort.** His June 2026 setup used medium effort. [matt-pocock-s-agentic-engineering-workflow-just-copy-him @ 00:24:00] In July 2026 he advised starting at the lowest. [your-effort-level-is-too-damn-high @ 00:00:50]

## Applying this

- *Inference:* When designing an agent app, decide first whether a workflow with a bounded AI step would do, since Matt's stated preference is predetermined steps unless the task is open-ended.
- *Inference:* Use his hallucination triage as a checklist: check the context for the missing fact before blaming the model.
- *Inference:* For a task that fits in one session, run a grilling round and implement directly; reserve specs, tickets and Wayfinder for multi-session work.
- *Inference:* Before adding to CLAUDE.md or AGENTS.md, ask whether the fact is discoverable in the repo, whether a hook or lint rule could enforce it, and whether a skill would pull it in only when needed.
- *Inference:* Size tickets to one fresh context window, slice vertically, and put the test seams in the spec.
- *Inference:* Treat the quoted token thresholds as dated examples and re-measure them for the model in use.
- *Inference:* Keep implementation and review in separate contexts, with the standards file read only by the reviewer.
- *Inference:* Do not take a skill from his repository as authoritative for his current view without checking its change date, since he revised several within weeks.

## Caveats

- Almost all video evidence is condensed review notes of auto-captioned talks, not verbatim transcripts. Thresholds, token counts, prices and product behaviour are the speaker's statements at recording time and are not independently checked.
- Dates are publication dates of the video or the file modification dates of the skills, not necessarily when he formed the view. The AI Hero pages are undated and carry only last-modified signals.
- The skills repository text is a snapshot from October 2026. Skills change quickly, and some guidance here may already be superseded.
- Guest views (Horthy, Martin and others) are attributed to them, not to Matt, and are used here only as context for his responses.
- This file does not cover his TypeScript-specific AI work (types around the AI SDK), his course business, or personal-software projects beyond a brief mention.
