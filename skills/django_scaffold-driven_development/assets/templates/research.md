# Research: <Project> Scaffold

## Candidates

<!-- 2–3 production-quality projects, same stack (Django + DRF + PG, React or
     Vue). Real code, not tutorials. Record repo + revision + license. -->

| Project | Repo / revision | License | Why relevant |
|---|---|---|---|
| <name> | <url> @ <tag/commit> | <license> | <reason> |

## Findings

<!-- Per candidate: concrete evidence from reading the code. No star scores. -->

### <Candidate A>

| Question | Evidence |
|---|---|
| Top-level layout | <directory tree> |
| App/module partitioning | <apps + import direction> |
| Frontend location | <separate package / Django-bundled> |
| settings organization | <one file / split> |
| API layer organization | <urls/views/serializers/services placement> |
| Auth integration | <how IdP/OIDC is wired> |
| compose / deployment shape | <services, networks> |

- Borrow: <pattern and where it will go in this project>
- Not applicable: <pattern and why this spec's constraints exclude it>

### <Candidate B>

<!-- same table -->

## Chosen Architecture

<!-- The directory layout proposal for THIS project, prose + tree, with each
     borrowed pattern attributed to its source candidate. This section is the
     input to docs/uml/component.mmd. -->

```text
<proposed tree>
```

- <decision> — from <candidate>
- <decision> — because <spec constraint>
