# Matt Pocock brain

**Status:** complete, published. Extraction covered 337 YouTube sources reviewed end to end, alongside 50 supporting records. The five knowledge files and the top-level files are written, and evals are in [evals/](evals/).

Matt Pocock teaches TypeScript through Total TypeScript and, since 2026, AI-assisted engineering through AI Hero. He describes about six years as a voice and singing coach before front-end work, and says his TypeScript courses became the main part of his income. [ai-skills-with-matt-pocock § Detailed source notes] This brain packages what his published work says and how he reasons, so an agent can explain his TypeScript guidance, his agent workflow and the skills he publishes, with dated positions and citations.

**Distinguishing question:** what does he do at the point where a developer is stuck, with a type error, a fuzzy plan or a full context window, and what feedback makes the next step obvious?

## Not an impersonation

This brain reports what Matt Pocock's published work says, cited to the corpus. It never speaks as him, claims his endorsement, or fills a gap with an opinion he has not published. Where the sources are silent, say so. Most video evidence is reviewer-written notes of auto-captioned talks, and his positions changed often, so every position carries a date.

## Files

| File | Holds |
| --- | --- |
| [AGENTS.md](AGENTS.md) | Short instructions for an agent consulting this brain |
| [PRINCIPLES.md](PRINCIPLES.md) | How he reasons about code, agents, TypeScript and teaching |
| [TECHNIQUES.md](TECHNIQUES.md) | Recurring techniques: the agent workflow, skills, testing, TypeScript patterns |
| [TRADEOFFS.md](TRADEOFFS.md) | Choices he describes with their gains and costs, and positions that changed |
| [SOURCES.md](SOURCES.md) | Corpus summary, licenses, weighting, citation key, and the source index |
| `knowledge/<topic>.md` | Per-topic reference, cited to the corpus |
| `corpus/ai/` | The 20 MIT-licensed skill files, with [Matt Pocock's license](corpus/ai/LICENSE-mattpocock-skills.md) |
| `evals/` | Evaluation questions and graded results |
| `reports/deep-modules/` | Worked report applying his deep-modules method |

## Coverage

| Topic | Knowledge file | Covers |
| --- | --- | --- |
| TypeScript type system | [knowledge/typescript-types.md](knowledge/typescript-types.md) | Generics, inference, utility, conditional and mapped types, `satisfies`, narrowing, discriminated unions, branded types, `any` versus `unknown` |
| TypeScript in practice | [knowledge/typescript-practice.md](knowledge/typescript-practice.md) | tsconfig, packages and monorepos, validation, React, errors, migration, tooling |
| AI engineering | [knowledge/ai-engineering.md](knowledge/ai-engineering.md) | LLM fundamentals, agents, the grill-spec-tickets-implement-review flow, testing and review with agents, writing for agents |
| Codebase design | [knowledge/codebase-design.md](knowledge/codebase-design.md) | Deep modules, seams, gray boxes, glossaries and ADRs, architecture review |
| Teaching and DX | [knowledge/teaching-and-dx.md](knowledge/teaching-and-dx.md) | Course design, explaining types, workshops and streams, developer experience, tool opinions |

## Scope and collection

The research covers work published from 2021-10-20 through 2026-10-20. YouTube is the primary stream: Videos, Shorts and Live tabs, deduplicated by video ID and checked against actual upload dates. Articles and licensed skills supplement it. Retrieval dates are never used as publication dates.

Source notes for the videos and articles are kept privately; [SOURCES.md](SOURCES.md) indexes every source with its original URL, and citations resolve through it.

## Rights and evidence

Source notes and full source text are different artifacts. Full text is published here only where a license has been verified, which means the 20 MIT-licensed skills in `corpus/ai/`. Everything else is cited by original URL with limited quotation, and its notes are kept privately. Working transcripts are not published, and their existence is not a redistribution grant. SOURCES.md records each source's license and scope.

## How it was checked

Every video was reviewed end to end: 92 by one designated reviewer and the rest by a Haiku review wave with Sonnet spot checks, so some notes are compressed and a missing topic is not evidence he never addressed it. Evals check accuracy, citation faithfulness and refusal to invent positions; see [evals/results-2026-10-24.md](evals/results-2026-10-24.md).
