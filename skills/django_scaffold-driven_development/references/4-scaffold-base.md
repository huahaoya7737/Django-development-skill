# Center 2c — Scaffold: Code Base

Generate the project skeleton and stub code from the confirmed UML. The base
is mechanical: every element comes from a diagram; nothing is invented here.

## Entry

- The human has confirmed the full UML set (`references/3-scaffold-uml.md`).

## The base contains

### Project skeleton (from component + deployment diagrams)

```text
<project>/
├── manage.py
├── pyproject.toml            # uv-managed, deps: django, djangorestframework,
│                             # psycopg, environment/oidc client lib (if IdP chosen)
├── compose/
│   └── compose.yml           # Django + PG (+ frontend if separated, + IdP stub),
│                             # per deployment diagram
├── config/                   # project package
│   ├── settings/
│   │   ├── base.py           # shared settings; DRF, PG, apps registry
│   │   ├── local.py
│   │   ├── test.py
│   │   └── production.py     # IdP/OIDC configuration slots (env-driven)
│   ├── urls.py               # route registration; includes API prefixes per
│   │                         # component diagram
│   ├── wsgi.py
│   └── asgi.py
├── apps/
│   └── <domain>/             # one per component-diagram app
│       ├── apps.py
│       ├── models.py         # class stubs from class diagram (see below)
│       ├── serializers.py    # serializer stubs from class diagram
│       ├── services.py       # service class stubs from class diagram
│       ├── selectors.py      # query-side stubs (if in class diagram)
│       ├── api/
│       │   ├── urls.py       # routes from the first-slice sequence diagram
│       │   ├── views.py      # view stubs from the sequence diagram
│       │   └── permissions.py
│       ├── migrations/       # empty (no fields yet → no migrations)
│       └── tests/
├── frontend/                 # if separated (React or Vue per specs)
│   ├── package.json
│   ├── vite.config / vue config
│   ├── src/
│   │   ├── main.*            # framework bootstrap only
│   │   ├── api/              # API client layer: module per endpoint domain
│   │   │                     # with functions matching the sequence diagram
│   │   │                     # endpoints; typed (TS) or typed (Vue+TS)
│   │   └── app shell only    # no pages, no components, no routes
├── .github/workflows/ci.yml  # lint, typecheck, format, import checks
└── docs/
    ├── spec.md
    ├── research.md
    └── uml/*.mmd
```

### Stub depth — the critical rule

The base is **stubs all the way**:

| From diagram | In base |
|---|---|
| Class (from class diagram) | `class Task(models.Model):` with attributes whose types are declared, `pass` — **or** attributes omitted and a comment `# fields per class diagram` when fields are development work. Attributes appear in code only if the spec named them and the human confirmed. |
| Method (from class diagram) | `def create(self, *, title: str) -> Task:` with signature, docstring `"""Per sequence diagram <slice>."""`, body `raise NotImplementedError` |
| Endpoint (from sequence diagram) | `urls.py` path entry + view function/class with signature, no logic; serializer class name from class diagram, fields empty |
| Frontend API call | client function with typed signature matching the endpoint, body throws / returns placeholder type |
| State machine (from state diagram) | a `STATES`/`TRANSITIONS` constant on the model stub; no enforcement logic |

One file per class. One route per sequence-diagram message to the system
boundary. **If it is not on a diagram, it is not in the base.**

## The base does NOT contain

- Model fields beyond spec-named attributes (no `created_at`, no FKs, no
  constraints unless the class diagram has them)
- Any method body beyond `raise NotImplementedError`
- Any serializer fields
- Any frontend page, component, router, or store
- Tests (none — all tests are per-task in development)
- Fixtures, migrations, seed data
- Any feature "to make it realistic"

The quality bar for the base: `uv sync` (and `npm install` when separated)
succeeds, the Django project boots (`manage.py check` passes), lint/typecheck
pass, and **nothing else works** — that is the point. No tests exist in the
base; every test is written during development, per task, RED first.

## Static checks allowed during scaffold

- `uv sync`, `npm install` / `npm ci` — dependency installation
- `manage.py check`, `manage.py makemigrations --check --dry-run` (with no
  models having fields, this stays empty)
- lint / typecheck / format
- `docker compose config` (validate topology, do not start containers)

Forbidden: runserver, any server start, pytest, `migrate`, database
connections, building frontend business features.

## Generation procedure

1. Generate the directory tree from the component diagram.
2. Generate settings/compose/CI from the deployment diagram.
3. Generate one stub file per class-diagram class; stub methods raise
   `NotImplementedError`.
4. Generate `urls.py` + view stubs from the first-slice sequence diagram.
5. Generate the frontend API client functions from the same sequence diagram.
6. Run the allowed static checks; fix failures.
7. **Hard stop.** Show the human:
   - the doc inventory (spec, research, UML files)
   - the code inventory (tree + one-line summary per file)
   - how each generated element traces to a diagram element
8. The human edits docs and base code directly. The agent does not touch
   anything until the human says continue.
9. On resume, **re-read everything from disk** (rule 4 of SKILL.md). Files
   win over memory. Report mismatches, then transition to development.

## Human edits and the freeze

After human confirmation, the base is frozen. During development:

- Filling stubs is always allowed (that is the development center's job).
- Changing the skeleton — adding/removing an app, moving files, altering
  settings structure or compose topology — requires the UML → base → human
  loop: update the diagram first, regenerate the affected part, human
  confirms again. It is never a silent development-time refactor.

## Red flags

| Thought | Correction |
|---|---|
| "I'll add a helper module; it's obvious." | Not on a diagram → not in the base. |
| "A `created_at` field is standard." | Fields come from the spec/class diagram or not at all. |
| "Let me implement the first view so it works." | Stub. Implementation is development. |
| "The human will want tests scaffolding." | No tests in the base. Tests are per-task in development, RED first. |
| "I'll quietly regenerate after human edits." | Never. After edits, read from disk and report differences. |
