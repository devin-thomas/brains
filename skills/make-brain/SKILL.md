---
name: make-brain
description: Start or continue building an expert brain for a person in brains-internal. Use only when the user invokes /make-brain.
---

# Make brain

Build a **brain**, a source-grounded Markdown package of how one expert reasons, in Devin's private `devin-thomas/brains-internal` repo. Find a local clone (conventionally `dev/resources/brains-internal`), or clone it there, then pull. Without access to that repo, the steps still apply to a brain kept in any repository that follows the same layout.

The repo is the authority; read these before acting and follow them over anything here:

- `AGENTS.md`: map of the repo.
- `PIPELINE.md`: how to extract and transcribe, the allowed tools and their cost rule, the corpus file format, and the citation format.
- `LIFECYCLE.md`: stages, and the rule that everything is built here and promoted to the public `brains` repo only once complete and approved by Devin.

## Steps

1. **Place the person.** Find them in `CANDIDATES.md` and `sources/<slug>.md` (slug: name in kebab case).
   - If they're new, ask Devin why this person and what question they'd answer that no other brain does. Add a `CANDIDATES.md` entry in the existing format with his reasons, and a `sources/<slug>.md` link catalog.
   - If a `brains/<slug>/` folder already exists, read its `corpus/MANIFEST.md` and resume where it stopped.
   - Done: the person has a roster entry, a source catalog, and a known stage.
2. **Set the scope** with Devin when a project drives it (e.g. the Guilty Gear priorities in `priorities/`). Record a scope decision that changes build order in that priorities file.
3. **Extract.** Hand the mechanical work to a cheaper extraction agent if your harness has subagents; otherwise do it yourself. That work is enumerating sources, fetching full text, and transcribing video. Its output is `brains/<slug>/corpus/` plus `MANIFEST.md`, which flags every judgement call instead of deciding it. You make those calls (whose words count, what to skip, rights) and record them in the manifest under *Lead decisions*.
   - Done: every source in the manifest is extracted, transcribed or skipped with a reason, and spot-checked against the live source.
4. **Synthesize.** Knowledge files may be drafted by the extraction agent. You write README, SOURCES, PRINCIPLES, TECHNIQUES, TRADEOFFS and AGENTS, following the `CANDIDATES.md` output format. Then run `python tools/check_citations.py <files>` and fix every real failure.
   - Done: the checker passes, apart from failures you've confirmed are false alarms.
5. **Evaluate.** Write `evals/` questions that cover facts, known corrections, refusal to invent, and third-person attribution. Run them with a fresh agent that sees only the brain, not the evals, and record the graded result in `evals/results-<date>.md`.
6. **Record.** Run `python db/sync.py` (it needs `DATABASE_URL` in `.env.local`; if that's missing, say so and continue). Set the expert's `status` in the database to match its stage. Commit and push brains-internal.
7. **Report** the stage reached, what's next, and anything waiting on Devin. Leave promotion to the public `brains` repo for his explicit approval.

## Rules that hold throughout

- Only free tools, plus Browserless and Apify. Credentials come from Devin's shared env file (see the `env` skill). Never print or commit a key.
- Never fabricate an expert's opinion. Report what the sources say; mark inference as inference.
- Plans for the brains product (plugin, MCP server) stay in brains-internal until built.
