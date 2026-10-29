---
name: django_scaffold-driven_development
description: Use when starting a new Python web project with Django + DRF + PostgreSQL and a React or Vue frontend, when the project needs spec-driven development with a UML-designed code base before any business code is written, or when a human asks for "specs first" or "scaffold before coding". Covers five centers - specs, scaffold, development, test/review, git - and applies when frontend/backend separation, IdP authentication, or PG-backed architecture decisions still need to be made.
---

# Django Scaffold-Driven Development

## Overview

Spec-driven development for Python web projects: **Django + DRF + PostgreSQL + React/Vue**.

Five centers, in order:

```text
specs → scaffold → development → test/review → git
```

The skill owns the first two centers completely and defines the rules for the last
three. No superpowers or external skill dependencies are required at runtime.

## Non-negotiable rules

1. **No business code before scaffold completes.** Scaffold produces the project
   skeleton, UML, and a code base of stubs. Implementation happens only in the
   development center.
2. **Scaffold must stop.** When the code base is generated, the agent stops,
   shows the doc and code inventory, and waits for human interaction. The human
   edits docs and base code directly. Only an explicit human "go" resumes work.
3. **Files are the single source of truth.** No version history, no change logs,
   no state files, no hash ledgers are ever created. The current file content is
   all that exists. Edits take effect by **re-reading** files.
4. **Re-read on every center transition.** Before entering a new center, the
   agent re-reads specs, research, UML, and base code from disk. Never continue
   from memory. If something does not match what the agent remembers, the file
   wins and the agent reports the difference before proceeding.
5. **The base is frozen after human confirmation.** Development fills empty
   files and stubs; it never restructures the skeleton or changes
   `settings`/`urls`/`compose` layout. Changing the skeleton requires going
   back through UML → base → human confirmation, inside the scaffold center.
6. **Diagrams map to code, one-way.** Every class, method, endpoint, and state
   in the UML becomes a stub in the base. Nothing in the base exists without a
   diagram element. The diagram set therefore controls base size, and bloat is
   impossible by construction.
7. **Ask one question at a time** during specs. Clarify before deciding.

## Stack scope

- Backend: Django + DRF, PostgreSQL.
- Frontend: React or Vue — chosen during specs, not by the agent.
- Auth: IdP/OIDC expected; the choice of IdP and its placement are decided
  during specs or scaffold architecture, with human input.
- Separation: frontend/backend split vs Django-served frontend is a mandatory
  specs question.

## The five centers

| Center | What it produces | Human control point |
|---|---|---|
| specs | One spec document | Spec approved by human |
| scaffold | Research, UML set, code base | UML confirmed before base generation; **hard stop after base generation**; human edits docs + code; agent re-reads |
| development | Implementation of UML elements | Per-task TDD; base frozen |
| test/review | Verified tasks | Review rubric; full verification before completion |
| git | Integrated work | Branch decisions by human |

Load the reference for the center you are entering; do not hold all five in
context at once.

| Center | Reference |
|---|---|
| specs | `references/1-specs.md` |
| scaffold — research | `references/2-scaffold-research.md` |
| scaffold — UML | `references/3-scaffold-uml.md` |
| scaffold — base | `references/4-scaffold-base.md` |
| development / test/review / git | `references/5-dev-review-git.md` |

## What this skill never creates

- `.agent/state.json` or any approval/state tracking files
- Hash manifests, verification scripts, approval ledgers
- Version history or change logs for any doc
- `AGENTS.md`, gates, or approval records

Simplicity of the doc tree is a hard requirement. The docs directory contains
only: spec, research, and UML files.
