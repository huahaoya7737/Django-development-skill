# Center 2b — Scaffold: UML

Design the system as Mermaid diagrams before generating any code. The diagram
set is the direct input to the code base: every element in a diagram becomes
a stub; nothing outside the diagrams exists in the base.

## Format

Mermaid, stored as `.mmd` text files under `docs/uml/`.

- Human-readable: renders on GitHub without extra tooling.
- Text: diffable, editable by human and agent alike.
- No image files. If the human provides hand-drawn sketches, the agent reads
  them and transcribes into Mermaid.

## The diagram set

### Structure diagrams (decide what the project is)

| Diagram | File | Decides |
|---|---|---|
| Component | `docs/uml/component.mmd` | Module boundaries and dependency direction → **directory structure**. Components map to Django apps + frontend packages + external systems (PG, IdP). The authoritative source for the directory tree; no separate architecture md file is maintained. |
| Deployment | `docs/uml/deployment.mmd` | Runtime topology → compose files and environment split. Nodes: Django, PG, frontend (if separated), IdP. |
| Class | `docs/uml/class.mmd` | Core domain classes: names, attributes, method signatures, relationships → stub files for models/serializers/services. **Attributes are included when the spec names them** (with types); the rest of the class is signature-level only. |

### Behavior diagrams (decide how it behaves)

| Diagram | File | Decides |
|---|---|---|
| Use case | `docs/uml/usecase.mmd` | System boundary and actors → cross-checks the spec's user stories. Boundary + actors only; no flow detail. |
| Sequence | `docs/uml/sequence-<slice>.mmd` | One diagram per vertical slice. **Only the first slice is created during scaffold**; it defines the endpoint list and the service call chain for that slice. Later slices are added during development, one per feature, each confirmed by the human. |
| State | `docs/uml/state-<domain>.mmd` | Only for domain objects with a real lifecycle (states + legal transitions). Absent by default — if no domain object has a lifecycle, no state diagram exists. |

Activity diagrams: **not used by default.** Only when branching is too complex
for a sequence diagram, and only with human approval.

## Design rules

1. **Every diagram must answer "which lines of the base does this generate?"**
   A diagram that generates nothing is deleted.
2. **Specs drive class attributes.** An attribute appears in the class diagram
   iff the spec names it. Class methods are signatures only.
3. **First slice only.** The scaffold contains sequence diagrams for exactly
   one vertical slice — the thinnest end-to-end path from the spec. Additional
   slices are development-center work.
4. **No behavior in scaffold.** Method bodies, serializers' field logic,
   frontend components — none of these exist at scaffold time. The diagrams
   stop at signatures and flows.
5. **Diagram growth is human-gated.** The agent never adds a diagram or a
   diagram element silently. Any addition is proposed, then confirmed.

## Mermaid conventions

- One diagram per file, filename matching the table above.
- Component diagram: nodes are apps/packages/external systems; edges are
  dependencies with a label.
- Class diagram: `class` blocks with attribute types; method signatures
  without bodies; relationship edges with multiplicities.
- Sequence diagram: participants are concrete classes/modules from the class
  and component diagrams — never invented names.

## Order of work

1. Component diagram from the research architecture summary.
2. Human confirms component diagram (this fixes the directory structure).
3. Deployment diagram.
4. Use case diagram (cross-check against spec stories; report mismatches).
5. Class diagram (attributes from spec).
6. State diagram(s) if any domain object qualifies.
7. Sequence diagram for the first slice.
8. Human confirms the full UML set. Then, and only then, base generation
   (`references/4-scaffold-base.md`).

## Exit checklist

- [ ] Component diagram confirmed by human.
- [ ] Deployment, use case, class diagrams present; state diagrams present
      only where justified.
- [ ] Exactly one sequence diagram (first slice).
- [ ] Every class attribute traces to the spec.
- [ ] Every sequence participant traces to the class/component diagrams.
- [ ] Human confirmed the full diagram set before base generation.

## Red flags

| Thought | Correction |
|---|---|
| "I'll add state diagrams for completeness." | No lifecycle, no state diagram. |
| "Extra sequence diagrams help." | First slice only. Extra slices are development work. |
| "This method body is obvious, I'll sketch it." | Signatures only. Bodies are development. |
| "The component diagram misses a detail, code will fix it." | The diagram is the source. Fix the diagram. |
