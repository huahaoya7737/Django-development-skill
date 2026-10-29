# Center 1 — Specs

Produce one spec document. Nothing else. No code, no directories, no choices
made unilaterally by the agent.

## Entry

Two supported entry modes. Ask the human which one, or detect from context:

1. **Document input.** The human provides a requirement document. The agent
   reads it, restructures it into the spec format, and then drives a dialogue
   to fill gaps — missing acceptance criteria, unstated constraints, open
   questions.
2. **Dialogue.** No document. The agent asks one question at a time until the
   spec is complete.

In both modes the agent's job is the same: clarify, then write. The agent
never invents answers.

## Mandatory questions

These are asked for every project. Do not skip; do not bundle into one
question:

1. **Frontend/backend separation?** Separate frontend (React or Vue, own
   build, talks to DRF over HTTP) or Django-served (templates/SPA bundled by
   Django)? This changes deployment topology, scaffold structure, and the
   frontend base.
2. **Which frontend framework?** React or Vue. If separation answer is
   "Django-served", ask whether an SPA framework is bundled or server
   templates are used.
3. **Authentication model.** IdP/OIDC expected? Which IdP, or is the IdP
   unknown at this point? Which flows (authorization code, client credentials,
   device)? If unknown, record it as an open question — do not default
   silently.
4. **PostgreSQL specifics.** Hosted vs self-managed, if known. Multiple
   databases?

Everything else follows the spec template sections.

## Question discipline

- One question at a time. One decision per question.
- After each answer, record it in `## Clarifications` — the answer lives in
   the spec, not in chat history.
- `## Open Questions` must be empty before specs exit. An undecided
   significant decision blocks the transition to scaffold. Unknown IdP is
   acceptable only as an explicit, human-acknowledged open item with a note
   on where it will be resolved (scaffold architecture).
- When the human is unavailable, stop. Never proceed on assumption.

## Output

One file: `docs/spec.md` (dated copy if the human prefers:
`docs/spec-<YYYY-MM-DD>.md`).

Follow `assets/templates/spec.md` section by section. Every user story uses
INVEST. Every acceptance criterion is testable (EARS form: WHEN/IF … THE
SYSTEM SHALL …). NFRs are measurable, not adjectives.

## Exit checklist

- [ ] All mandatory questions answered or explicitly parked as open questions.
- [ ] Every story has acceptance criteria.
- [ ] NFRs are measurable.
- [ ] Open Questions is empty (or only items the human explicitly deferred
      with a resolution point).
- [ ] The human has read and approved the spec.

## Red flags

| Thought | Correction |
|---|---|
| "I can guess the auth model." | Ask. Wrong auth assumptions poison the scaffold. |
| "Separation doesn't matter yet." | It changes the directory layout — it is a specs question. |
| "I'll fill in the spec from the doc silently." | Restructure, then ask about every gap you found. |
| "Bundling three questions saves time." | One decision per question. |
