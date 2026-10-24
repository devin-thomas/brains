---
name: deep-modules
description: Find shallow modules in a codebase and deepen them, following Matt Pocock's documented method. Use when the user invokes /deep-modules, asks to deepen or de-slop a codebase, or wants pass-through modules, scattered rules or hard-to-test code turned into deep modules.
---

# Deep modules

Survey a codebase for **shallow** modules (an interface nearly as complex as its implementation) and turn the one the user picks into a **deep** module: a lot of behaviour behind a small interface, at one seam, tested through that interface. The method is Matt Pocock's, as recorded in the Matt Pocock brain; the agent is the sergeant who finds candidates, the user is the general who decides.

**Vocabulary.** Load the `codebase-design` skill first and use its terms exactly: module, interface, implementation, depth, seam, adapter, leverage, locality, the deletion test, and "two adapters make a real seam". When it is not installed, read sections 1 and 5 of the Deep Modules report in this repository instead: `brains/matt-pocock/reports/deep-modules/report.md` (https://github.com/devin-thomas/brains). When the user asks *why* Matt holds a position, cite that report or the brain, in the third person.

## 1. Scope

Take the user's direction if they gave one. Otherwise weight recent change: read `git log --stat` over the last few weeks and list the hot directories, widening only if changes are scattered. Read `GLOSSARY.md` and ADRs in the area when they exist.

Done when you can name the directories in scope and why each is hot.

## 2. Survey

Walk the scoped code (a sub-agent keeps the main context clean). Record friction against these questions, citing files:

- Where does understanding one concept mean bouncing between many small modules?
- Which modules have an interface nearly as complex as the implementation (pass-throughs, re-export helpers, wrappers that rename arguments)?
- Where were pure functions extracted for testability while the bugs live in how they are called?
- Where is one rule implemented in two places (front end and back end, two callers) that can drift?
- Where do coupled modules leak across their seams?
- What is untested, or testable only by mocking internals or reading the database behind the interface?

For every suspect, run the **deletion test**: if the module vanished, would complexity vanish (pass-through) or reappear across callers (earning its keep)? For each candidate, write the **whole interface**: types plus invariants, ordering rules, error modes, configuration and performance facts a caller must know. That list is the real size to shrink.

Done when every suspect has a deletion-test verdict and every candidate has its whole interface written down.

## 3. Report and stop

Write a self-contained HTML report to the OS temp directory, outside the repo, and open it. One card per candidate: files, problem, proposed deepening, benefits (locality, leverage, tests), a before/after sketch, and strength **Strong**, **Worth exploring** or **Speculative**. End with one top recommendation. Mark any candidate that contradicts an ADR, and include it only when the friction justifies revisiting the ADR.

Propose no interfaces yet. Ask the user which candidate to explore, then wait.

## 4. Design the interface with the user

For the chosen candidate, interview the user one decision at a time: constraints, dependencies, what goes behind the seam, which callers change, which tests survive. Shrink the interface with three questions: fewer methods? simpler parameters? more complexity hidden inside? When two shapes compete, design it twice (parallel sub-agents, compare on depth, locality and seam placement); `codebase-design`'s DESIGN-IT-TWICE.md has the pattern.

Seam rules:

- Put the seam as high as possible; one seam is ideal. Confirm it with the user.
- Add a port only when two adapters exist, usually production plus an in-memory fake. One adapter is indirection.
- Accept dependencies, return results, keep internal seams internal.
- Classify each dependency (in-process, local stand-in, owned remote, true external) with `codebase-design`'s DEEPENING.md to pick the test strategy.

Done when the user has agreed the interface (every caller-facing fact listed) and the seam.

## 5. Deepen

Write tests at the new interface first, asserting observable outcomes. Move the behaviour behind the interface, migrate callers, then delete the old shallow modules and the unit tests that targeted them (replace, don't layer). A test that must change when only the implementation changes is testing past the interface; rewrite it.

Treat critical code (money, auth, security) as reviewed code: the user still reads the implementation, not only the tests.

Done when callers use only the new interface, the old modules and their tests are gone, and the suite is green.

## 6. Record

Add any new concept name to `GLOSSARY.md`. Offer an ADR only for a rejected alternative that is hard to reverse, surprising without context, and a real trade-off, so later surveys do not re-suggest it. Report to the user: what was deepened, the interface before and after, tests replaced, and the next-strongest candidate from the report.
