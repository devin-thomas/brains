---
name: brain
description: "Ask an existing expert brain (e.g. Copetti, Korth) or a team of brains a question, answered from cited sources. Use when the user invokes /brain, asks what an expert's work says, or asks a brain team for help."
---

# Brain

Answer a question from one of Devin's **brains**: source-grounded Markdown packages of how a specific expert reasons. This is **ask mode**: you answer here, in this conversation.

Start a separate agent that works from a brain only when the user explicitly asks for one ("instantiate", "spin up an agent", "run it as an agent"). Then give that agent the brain's `AGENTS.md` as its instructions and the user's task; relay its cited answer.

## Find the brains

Two Git repos, both under `devin-thomas` on GitHub:

- `brains` (public): finished brains.
- `brains-internal` (private): brains in progress, the roster, and teams. Without access to it, use the public repo alone: finished brains only, and pick the brain by its README.

Look for a local clone first (conventionally `dev/resources/<repo>`; otherwise search the home directory). If missing, clone it there. Pull before answering so you read the latest revision.

Brains live at `brains/<slug>/`, where the slug is the person's name in kebab case (`rodrigo-copetti`). A promoted brain in `brains` wins over its internal copy.

## Resolve who is asked

- **A named person:** use `brains/<slug>/`.
- **A team** (e.g. `guilty-gear`): the members and why each is on it are in `brains-internal/priorities/<team>.md`. Ask each member that has a brain.
- **Nobody named:** match the question to each candidate's *distinguishing question* in `brains-internal/CANDIDATES.md`, and say which brain you picked and why.

If the person has a source catalog (`sources/<slug>.md`) but no `brains/<slug>/` folder, there is no brain yet. Say so and offer `/make-brain`. Answer from the catalog only when the user asks you to.

## Answer

Follow the brain's own `AGENTS.md`; it overrides anything here. The rules every brain shares:

1. Read `knowledge/` for the topic, then go to `corpus/` when you need the full passage.
2. Every factual sentence carries the brain's citation, `[corpus-slug § Heading › Subheading]` or `[slug @ hh:mm:ss]`, so the user can check it.
3. Keep three kinds of statement visibly apart: what the source says (cited); synthesis across cited passages; and *Inference:* for the user's own project.
4. Speak about the expert in the third person. Never write as them, and never claim their endorsement.
5. When the sources are silent, say "<Name>'s work doesn't cover this" and stop, then point to another brain whose area it is, if there is one.
6. Check the brain's caveats and corrections before relying on a fine detail.

For a team, answer per expert under their name, then a short synthesis. When they disagree, show the disagreement with both sides cited; don't blend it away.

**Done when:** every claim in the answer is cited or labelled *Inference:*, and every gap is named as a gap.
