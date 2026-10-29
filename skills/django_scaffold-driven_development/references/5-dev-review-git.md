# Centers 3–5 — Development, Test/Review, Git

After scaffold confirmation, these centers run the standard loop. Their rules
are inlined here; no external superpowers dependency is required.

## Entry

- Scaffold center completed: human edited docs + base, then said go.
- **Re-read rule in force:** before the first task, re-read spec, research,
  UML, and base code from disk. Files win; report mismatches.

## Center 3 — Development

### Task flow

Work in vertical slices. Each slice = one sequence diagram (the first one
exists from scaffold; each new slice starts by drawing its sequence diagram
and getting human confirmation — rule 5 of `3-scaffold-uml.md`).

Per task:

1. Take one stub (class, method, endpoint, or frontend API call) from the
   slice.
2. **RED:** write a failing test against the stub's promised behavior.
   Run it. Confirm it fails for the right reason.
3. **GREEN:** implement the stub — the minimum that passes. Body goes where
   the `NotImplementedError` was.
4. **REFACTOR:** clean up within the slice; rerun tests.
5. Commit (Conventional Commits, see git center).

### Base freeze rules

- Filling stubs: always allowed.
- Adding fields not in the class diagram: **no** — update the class diagram
  first (diagram edit → human confirmation → then code), or the field does
  not exist.
- Restructuring skeleton (new app, moved file, settings/compose layout):
  **no** — UML → base → human loop, inside a scaffold-mode task, never a
  silent refactor.
- Frontend pages/components: built per slice; the API client functions from
  the base are consumed, not rewritten.

### Stop conditions

Stop and ask the human when:

- The implementation requires something not on any diagram.
- A test cannot be written because the spec is ambiguous.
- Two diagram elements contradict each other.
- A stub's signature turns out wrong for its callers.

## Center 4 — Test/Review

### Per-task completion bar

Before declaring a task done:

1. New test exists, failed first, passes now.
2. Full suite green: `uv run pytest`, frontend tests, lint, typecheck,
   format — all pass.
3. The stub list is updated: the implemented element is no longer a stub.
4. No `NotImplementedError` remains in touched files (unless intentionally
   deferred, with a recorded reason in the task/commit message).

### Review rubric (applied to each completed slice, before merge)

| Area | Check |
|---|---|
| Spec compliance | Every acceptance criterion of the slice is covered by a test |
| Diagram compliance | Code matches class/sequence diagrams; deviations went through diagram-first updates |
| Base integrity | No skeleton restructure happened; only stubs were filled |
| Test quality | RED before GREEN; meaningful assertions; no skipped tests |
| API contract | Endpoints match the sequence diagram; response shapes match serializer stubs |
| Security | No secrets in code/logs; auth checks at the boundary per component diagram |

Findings are fixed or explicitly accepted by the human. Never merge with an
unresolved finding.

### Verification before claiming completion

Never claim "done/working/fixed" without running the commands and reading
the output. Evidence first, claims second. If the suite was not run, the
work is not done.

## Center 5 — Git

### Branch model

- One branch per feature/slice: `<type>/<short-slug>` (e.g.
  `feat/task-create`), short-lived, from the default branch.
- Rebase before review; squash or merge per the human's preference — ask
  once, record the answer in the repo's contributing notes if needed.
- Never force-push a shared branch.

### Commits

- Conventional Commits: `feat`, `fix`, `test`, `refactor`, `docs`,
  `chore`, `build`, `ci`.
- One logical change per commit; every commit leaves the suite green.
- Body explains why, not what.

### Completion of a branch

When a slice is implemented, reviewed, and green:

1. Run the full verification one final time on the final tree.
2. Present the human the integration choice: merge locally / push and open
   a PR / keep the branch. The decision is the human's.
3. After integration, delete the feature branch.

## Red flags

| Thought | Correction |
|---|---|
| "I'll fix the diagram after the code works." | Diagram first. Always. |
| "The test can come later." | RED before GREEN, every task. |
| "It's just one field, no diagram update needed." | Fields come from the class diagram. Update it. |
| "Suite was green earlier." | Rerun on the tree you are about to integrate. |
| "I'll merge; human said ok yesterday." | Ask at the merge point, not from memory. |
