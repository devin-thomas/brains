# brains

Condensing the knowledge of great minds 🧠

A **brain** is a source-grounded Markdown package of how one expert reasons: their principles, techniques, tradeoffs and per-topic knowledge, drawn from their published work and cited claim by claim. Agents consult a brain the way you'd consult a well-annotated reference shelf, so they can explain why an approach makes sense rather than just what to do.

## What a brain is, and isn't

- **It captures reasoning.** Brains aim to preserve how an expert thinks: what they weigh, when they'd choose differently, and how their views have changed over time. Disagreements and caveats are kept, not flattened.
- **It cites everything.** Every factual sentence points at a source: a section of an article, or a timestamp in a video. A source index maps each citation to the original URL so anyone can check it.
- **It keeps three kinds of statement apart.**
  - What the source says, cited.
  - A synthesis drawn across several cited passages.
  - An inference about applying it to your project, always labelled as such.
- **It never impersonates.** A brain reports what someone's published work says, in the third person. It doesn't speak as them, claim their endorsement, or fill a gap with an opinion they haven't published. When the sources are silent, it says so.

## What's in a brain

| File | Holds |
| --- | --- |
| `README.md` | Scope, coverage and how the brain was checked |
| `AGENTS.md` | Instructions for an agent consulting the brain |
| `PRINCIPLES.md` | How the expert reasons |
| `TECHNIQUES.md` | Concrete methods they teach |
| `TRADEOFFS.md` | Choices they weigh, and the conditions behind them |
| `SOURCES.md` | Every source, with dates, licenses and original links |
| `knowledge/` | Dense, cited reference by topic |
| `evals/` | Test questions: facts, corrections, refusal to invent, refusal to impersonate |

Source text is published only where its license allows. Everything else is cited by link.

## How brains are built

A brain starts as a list of every source an expert has published: articles, books, talks, videos. Each source is extracted or transcribed, and transcripts are corrected against a glossary of the expert's own terms. The knowledge files are then written from that corpus, checked by a script that confirms each citation resolves to the section it names, and tested by a fresh agent that sees only the brain. A brain is published here once it passes.

## Using brains

Two skills in [`skills/`](skills/) work with any agent harness that reads `SKILL.md` files:

- [`brain`](skills/brain/SKILL.md): ask one brain, or a team of them, a question and get a cited answer.
- [`make-brain`](skills/make-brain/SKILL.md): build a new brain with the same process.

Or point an agent at a brain's `AGENTS.md` and let it work from there.

## Brains

- [Matt Pocock](brains/matt-pocock/README.md): TypeScript, codebase design, AI-assisted development.
  - Extra: the [Deep Modules report](brains/matt-pocock/reports/deep-modules/report.md) and the [`deep-modules`](skills/deep-modules/SKILL.md) skill built from it.

## License

MIT for this repository's own writing. Third-party source text keeps its original license, recorded in each brain's `SOURCES.md`.
