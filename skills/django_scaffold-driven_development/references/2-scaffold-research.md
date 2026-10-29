# Center 2a — Scaffold: Research

Before inventing a directory layout, read real high-quality projects with the
same stack. The goal is an evidence-based architecture, not a copy of one
project.

## Entry

- An approved spec (specs center exit).

## Select candidates

2–3 production-quality projects:

- Same stack: Django + DRF + PG, with React or Vue frontend matching the
  specs choice.
- Real, readable code — not blog posts, not tutorials, not empty starters.
- Prefer different sizes/topologies so the comparison has range.
- Record repo, revision (tag/commit), and license for each.

If network access is unavailable or the human has trusted references, ask the
human for reference projects instead of guessing.

## Inspect

For each candidate, read the code and answer:

| Question | Evidence |
|---|---|
| Top-level layout | directory tree |
| App/module partitioning | app names, import direction |
| Where does the frontend live | separate package vs Django-bundled |
| settings organization | one file vs split (base/local/test/production) |
| API layer organization | urls/views/serializers/services placement |
| Auth integration | how IdP/OIDC is wired (if present) |
| compose / deployment shape | services, networks |

No scoring. Write per-candidate: what to borrow, what does not apply given
this spec's constraints.

## Output

One file: `docs/research.md` following `assets/templates/research.md`.

The research must end with a **chosen architecture summary**: the directory
layout proposal for this project, described in prose + tree, with each
borrowed pattern attributed to its source candidate. This summary is the
input to the component diagram.

## Exit checklist

- [ ] 2–3 candidates with exact revisions.
- [ ] Per-candidate borrow / not-applicable notes.
- [ ] Architecture summary written (directory layout + attribution).
- [ ] Human has reviewed the research and the architecture direction.

## Red flags

| Thought | Correction |
|---|---|
| "I know the Django best layout." | The research is about this project's constraints, not general knowledge. |
| "One candidate is enough." | One candidate gives a copy, not a decision. |
| "Research can be skipped, layout is standard." | Human asked for research before architecture. It is not optional. |
