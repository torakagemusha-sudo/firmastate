# FirmaState

A governed graph-native IDE for authoring, validating, simulating, and deterministically exporting executable state machines with capability-typed effects.

### Now Live !!

[![Website Status](https://img.shields.io/website?url=https%3A%2F%2Ffirmastate-ide.app&label=firmastate-ide.app&up_message=online&down_message=offline)](https://firmastate-ide.app)

### @ https://firmastate-ide.app

**Note: Paid subscriptions are not live yet, have a look around in free mode and feel free to email firmastate@torafirma.com or support@torafirma.com for any support related queries. For commercial enquiries please direct them to admin@torafirma.com**

**Lots of good things to come....**

> Visual graph. Canonical DSL. Controlled execution. Deterministic TypeScript export.

---

## Table of contents

1. [Why FirmaState](#why-firmastate)
2. [Core capabilities](#core-capabilities)
3. [UI walkthrough](#ui-walkthrough)
4. [Concepts in depth](#concepts-in-depth)
   - [The machine model](#the-machine-model)
   - [Graph-native authoring](#graph-native-authoring)
   - [Canonical DSL](#canonical-dsl)
   - [Capabilities and the authority model](#capabilities-and-the-authority-model)
   - [Effects](#effects)
   - [Controlled simulation](#controlled-simulation)
   - [Deterministic TypeScript export](#deterministic-typescript-export)
   - [Import paths](#import-paths)
5. [Architecture overview](#architecture-overview)
6. [Design principles](#design-principles)
7. [Getting started](#getting-started)
8. [DSL reference](#dsl-reference)
9. [Roadmap](#roadmap)
10. [FAQ](#faq)
11. [Glossary](#glossary)
12. [Contributing](#contributing)
13. [Contact](#contact)

---

## Why FirmaState

Most workflow and state tools split the truth across diagrams, handwritten code, runtime behaviour, and undocumented side effects.

A diagram is drawn in one tool. Code is written in another. The simulation is a separate harness. Effects — network calls, file writes, external service invocations — live inside handler functions that are never formally described. When any one layer drifts from the others, bugs and audit gaps follow invisibly.

FirmaState makes the machine itself the source of truth:
- author visually or in DSL
- inspect and validate structure
- simulate in a controlled execution environment
- declare capabilities and effects explicitly
- export deterministic TypeScript from the same machine model

Every view — the graph, the DSL, the simulation trace, the exported code — is derived from one canonical model. Edit any view and the rest update. Nothing drifts.

---

## Core capabilities

- **Graph-native authoring** for states, transitions, and machine structure
- **Canonical DSL projection** synchronized with the graph
- **Controlled execution** for simulation, traces, and runtime inspection
- **Capability and effect authority model** for governed execution
- **Deterministic TypeScript export** with validation and provenance metadata
- **Import paths** for JSON / DSL and template-based or AI-assisted generation

---

## UI walkthrough

### Author machines visually in the graph editor
<img width="2560" height="1265" alt="27" src="https://github.com/user-attachments/assets/c932aa22-9a06-467a-8966-6b05377e6646" />

The graph editor is the primary authoring surface. States appear as nodes; transitions appear as directed edges labelled with events. Compound states, parallel regions, history nodes, and final states are all first-class constructs. Drag to reposition, double-click to rename, right-click for context actions.

---
### Declare authority with capabilities and effects
<img width="2554" height="1269" alt="24" src="https://github.com/user-attachments/assets/c9306515-cfb5-4393-b29e-42800bc99363" />

Every state and transition that touches the outside world must declare a capability. Capabilities are named, typed permissions that must be granted before the machine is allowed to execute. Effects — the concrete actions performed — are bound to those capabilities. This panel is where capabilities are defined and effects are attached.

---
### Execute safely in a controlled simulation environment
<img width="2556" height="1272" alt="26" src="https://github.com/user-attachments/assets/f63f796b-6b14-40be-b5b7-cc9a6cb1ca35" />

The simulation environment steps through the machine event by event. You fire events, observe the active state set, inspect the context snapshot, and read the full execution trace. Effects are intercepted and logged rather than executed, keeping the simulation side-effect free.

---
### Export deterministic TypeScript with provenance
<img width="2556" height="1262" alt="25" src="https://github.com/user-attachments/assets/84f5854b-aa32-41fe-8884-0b5143763548" />

The export panel produces TypeScript that is structurally identical every time for the same machine model. The output includes a provenance block — a hash of the canonical model, the export timestamp, and the tool version — so generated files are traceable back to the machine that produced them.

---
### Edit or inspect the canonical DSL
<img width="2553" height="1268" alt="23" src="https://github.com/user-attachments/assets/16b6aa2b-056f-4442-891a-cbde025c3bde" />

The DSL editor and the graph editor are live mirrors of each other. Edit the DSL to add a state or rename an event and the graph updates immediately. Edit the graph and the DSL reflects the change. The DSL is also the interchange format: copy it out, version it, diff it, or paste it into a new machine.

---

## Concepts in depth

### The machine model

FirmaState is built around a formal state machine model that extends classical hierarchical state machines (HSMs / Statecharts) with an explicit capability and effect layer.

A machine consists of:

| Concept | Description |
|---|---|
| **States** | Named nodes that the machine can occupy. Can be atomic, compound (containing child states), parallel (occupying multiple regions simultaneously), or final. |
| **Transitions** | Directed edges from a source state to a target state, labelled with an event and an optional guard condition. |
| **Events** | Named signals that drive transitions. Events carry an optional typed payload. |
| **Context** | Typed data carried alongside the active state set. Updated by actions on transitions. |
| **Actions** | Pure functions executed on transition entry, exit, or the transition itself. Actions update context but do not produce external effects. |
| **Capabilities** | Named permissions that the machine declares it needs. A capability must be granted before execution to allow the effects bound to it. |
| **Effects** | Described side effects bound to capabilities. Effects are declared, not inlined — they describe what will happen, and the runtime is responsible for execution. |
| **Guards** | Boolean conditions evaluated against the current context and event payload to determine whether a transition fires. |
| **History nodes** | Shallow or deep history pseudostates that restore the previously active substate when a compound state is re-entered. |

### Graph-native authoring

The graph is not a documentation artifact generated from code — it *is* the primary edit surface. Changes made in the graph are immediately reflected in the canonical model, not accumulated as a series of patches applied later.

This matters because graph editing has a distinct affordance: spatial layout communicates intent, compound state nesting is visually obvious, parallel regions are explicit, and the shape of the machine is legible at a glance. Authors work in the medium that matches how they reason about state.

Features of the graph editor:
- Add states by double-clicking the canvas
- Add transitions by dragging from a state's output port to a target state
- Nest states by dragging one inside another to create compound states
- Create parallel regions by adding a parallel state node and adding child regions
- Add history nodes from the node palette
- Rename any element with a double-click
- Context menu on any node or edge for delete, duplicate, set as initial, set as final, and more
- Minimap for large machines
- Zoom and pan

### Canonical DSL

The DSL is a human-readable, diff-friendly text representation of the machine model. It is always in sync with the graph — editing either surface updates both.

The DSL serves several purposes:
- **Versioning** — the DSL is text, so it diffs cleanly in git
- **Review** — a state machine in DSL form is readable in a pull request
- **Import / export** — machines can be saved, shared, and loaded as DSL
- **Programmatic generation** — the DSL can be produced by scripts or AI assistants and then loaded into FirmaState

The DSL is the source of truth for the interchange format. The graph is a rendered view of it.

### Capabilities and the authority model

Most state machine tools describe *what* the machine does but not *what it is allowed to do*. The authority model in FirmaState adds a formal permission layer.

A capability is a declaration: "this machine requires permission to perform X". Examples:
- `capability network.fetch` — permission to make network requests
- `capability storage.write` — permission to write to persistent storage
- `capability ui.notify` — permission to push notifications to the user interface

Capabilities must be explicitly **granted** before a machine instance can execute. A machine that declares `network.fetch` but runs in a context where that capability has not been granted will not execute the associated effects — the machine can still transition, but the effect is withheld.

This has several practical consequences:
- **Testing without mocks** — run a machine in simulation without granting any capabilities; effects are logged but not executed
- **Progressive trust** — grant only the capabilities a machine needs in each environment
- **Audit surface** — all required capabilities are declared in the model and visible in the DSL and graph
- **Least-privilege by default** — capabilities must be granted; they are never assumed

### Effects

Effects are the described side effects of a machine. They are attached to transitions or state entries/exits and are bound to a capability.

An effect is a declaration with a name, the capability it requires, and a typed input shape. It is not an inline function. The actual execution logic lives in an effect handler registered at runtime, separate from the machine model.

This separation means:
- The machine model remains pure and serialisable
- Effect handlers can be swapped (e.g. replace a real network handler with a stub for testing)
- The simulation environment intercepts effects without executing them
- The exported TypeScript includes typed effect signatures, not inline logic

### Controlled simulation

The simulation environment is a sandboxed execution context for a machine. It:

1. Loads the machine model
2. Starts the machine in its initial state
3. Accepts events fired by the user
4. Steps through transitions one event at a time
5. Records a full trace: events, transitions taken, context snapshots, effects intercepted
6. Renders the active state set on a live copy of the graph

Effects are **intercepted**, not executed. When a machine would trigger an effect, the simulation records the effect call with its arguments and marks it in the trace, but does not call any external handler. This makes simulation side-effect free and reproducible.

Guards and actions that reference context work normally — context is carried, updated, and inspected at every step.

The trace is exportable and can serve as a test fixture: record a simulation, export the trace, use it to assert expected behaviour in a test suite.

### Deterministic TypeScript export

FirmaState exports TypeScript that is structurally determined by the machine model. Given the same model, the export is always identical — same types, same structure, same ordering.

The exported output includes:

- **State type** — a discriminated union of all state names
- **Event type** — a discriminated union of all events with their payload types
- **Context type** — the typed context interface
- **Capability declarations** — the capability strings the machine requires
- **Effect signatures** — typed function signatures for each declared effect
- **Transition table** — the full transition map as a typed data structure
- **Provenance block** — model hash, export timestamp, tool version

The provenance block makes every exported file traceable. If you have a TypeScript file and want to know which machine model produced it, the provenance block tells you.

The export is not a code generator that writes arbitrary logic. It exports the structure and types of the machine; implementation details (action functions, effect handlers) are filled in by the developer against a stable typed contract.

### Import paths

Machines can be created from existing sources:

- **DSL import** — paste or load a FirmaState DSL file
- **JSON import** — import a machine serialised as JSON (e.g. from a previous export or programmatic generation)
- **Template library** — start from a curated set of common machine patterns (authentication flows, multi-step forms, async request handling, etc.)
- **AI-assisted generation** — describe the machine in natural language and let an AI assistant generate an initial DSL, which is then loaded into FirmaState for review and editing

All import paths produce the same canonical model. There is no "AI-generated machine" that behaves differently from a hand-authored one — the source does not affect the model.

---

## Architecture overview

FirmaState is built around a single canonical model layer. All surfaces — graph, DSL, simulation, export — are projections of that model.

```
┌─────────────────────────────────────────────────────────────┐
│                        FirmaState IDE                        │
│                                                             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────┐  │
│  │  Graph editor│  │  DSL editor  │  │  Capabilities /  │  │
│  │  (visual)    │  │  (text)      │  │  Effects panel   │  │
│  └──────┬───────┘  └──────┬───────┘  └────────┬─────────┘  │
│         │                 │                   │             │
│         └─────────────────┴───────────────────┘             │
│                           │                                  │
│              ┌────────────▼────────────┐                    │
│              │    Canonical machine    │                    │
│              │         model           │                    │
│              └────────────┬────────────┘                    │
│                           │                                  │
│         ┌─────────────────┴───────────────────┐             │
│         │                                     │             │
│  ┌──────▼───────┐                  ┌──────────▼──────────┐  │
│  │  Simulation  │                  │  TypeScript export  │  │
│  │  engine      │                  │  (deterministic)    │  │
│  └──────────────┘                  └─────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

**Model layer** — the in-memory representation of the machine. All mutations go through the model layer; views are derived.

**Graph editor** — a visual projection of the model. Edits emit model mutations.

**DSL editor** — a text projection of the model. Parses DSL on edit and emits model mutations.

**Capabilities / Effects panel** — a form projection of the capability and effect declarations in the model.

**Simulation engine** — a statechart interpreter that runs against the model. Effects are intercepted; context is live; traces are recorded.

**TypeScript export** — a deterministic code-generation pass over the model, producing typed TypeScript output with a provenance block.

---

## Design principles

- **Machine as authority** — the state machine model is the source of truth; no view or export is authoritative on its own
- **Multiple synchronized views** — graph, DSL, simulation, and export derive from the same machine; editing any view updates the model
- **Governed execution** — effects require explicit authority; capabilities must be declared and granted
- **Deterministic compilation** — identical machines produce identical outputs every time
- **Inspectable systems** — validation, traces, and constraints are first-class, not afterthoughts
- **Separation of structure and logic** — the machine model describes structure and behaviour; implementation details (action functions, effect handlers) are separate and typed
- **Least-privilege by default** — capabilities must be explicitly granted; nothing is assumed

---

## Getting started

FirmaState is currently available as a hosted preview. No installation is required.

1. Open **https://polish-pass-2.emergent.host** in your browser
2. Create a new machine or load a template from the template library
3. Author states and transitions in the graph editor or DSL editor
4. Declare any capabilities your machine requires in the Capabilities panel
5. Attach effects to transitions via the Effects panel
6. Run a simulation: fire events, step through transitions, inspect the trace
7. When satisfied, export deterministic TypeScript from the Export panel

The preview is available in free mode. Paid tiers (additional machine slots, team collaboration, private machines, and extended simulation history) are planned and will be announced via the contact addresses below.

---

## DSL reference

> The DSL specification is under active development and may evolve before the first stable release. This section describes the current shape of the language.

### Machine declaration

```
machine <name> {
  initial: <state-name>
  context: { <key>: <type> }
  capabilities: [ <capability-name>, ... ]

  <states>
}
```

### State declaration

```
state <name> {
  on {
    <event-name> [guard: <expression>] -> <target-state> {
      actions: [ <action-name>, ... ]
      effects:  [ <effect-name>, ... ]
    }
  }
  entry: [ <action-name>, ... ]
  exit:  [ <action-name>, ... ]
}
```

### Compound state

```
state <name> {
  initial: <child-state-name>

  state <child-name> { ... }
  state <child-name> { ... }
}
```

### Parallel state

```
parallel <name> {
  region <region-name> {
    initial: <state-name>
    state <name> { ... }
  }
  region <region-name> {
    initial: <state-name>
    state <name> { ... }
  }
}
```

### Final state

```
final <name>
```

### History node

```
history <name> [deep]
```

### Capability declaration

```
capability <name> {
  description: "<human-readable description>"
}
```

### Effect declaration

```
effect <name> {
  capability: <capability-name>
  input: { <key>: <type> }
}
```

---

## Roadmap

The following areas are under active development. Priority and scope may change.

### Near term

- [ ] Stable DSL v1 specification and parser
- [ ] Shareable machine URLs (read-only link to a machine)
- [ ] Machine versioning and diff view
- [ ] Export to XState v5 format
- [ ] Simulation trace export (JSON and Markdown)

### Medium term

- [ ] Team workspaces and machine sharing
- [ ] Paid tier: private machines, extended simulation history, team collaboration
- [ ] Git integration — commit DSL directly to a repository from the IDE
- [ ] Guard expression editor with type-aware autocomplete
- [ ] Action function scaffolding — generate typed stubs for all declared actions
- [ ] Effect handler scaffolding — generate typed stubs for all declared effects
- [ ] Embedded documentation on states and transitions

### Longer term

- [ ] Open-source runtime library (independent of the IDE)
- [ ] Self-hosted / on-premises deployment option
- [ ] Plugin / extension API
- [ ] Machine testing framework — replay traces as assertions
- [ ] Visual diff of two machine versions
- [ ] AI-assisted machine validation ("does this machine match your described intent?")

---

## FAQ

**Is FirmaState a wrapper around XState?**
No. FirmaState has its own machine model, simulation engine, and export pipeline. The export format can target XState v5 (on the roadmap), but the IDE itself is independent.

**Can I use the exported TypeScript in any project?**
Yes. The exported TypeScript is plain TypeScript with no runtime dependency on FirmaState. It exports types and a transition table. You bring your own executor (or use a standard statechart interpreter like XState).

**How does deterministic export work?**
The export is driven by a single deterministic pass over the canonical model. The output ordering is derived from the model structure, not from insertion order or random identifiers. Given identical model content, the output is always the same. The provenance hash is computed from the canonical serialisation of the model before export.

**What happens if I grant only some capabilities?**
The machine runs, transitions fire, context updates, actions execute. Effects bound to capabilities that were not granted are skipped. The simulation and execution logs mark skipped effects explicitly.

**Is the DSL a new language I have to learn?**
The DSL is intentionally minimal and mirrors the structure of the graph. If you can read the graph, you can read the DSL. If you find the DSL verbose, use the graph editor. They are always in sync.

**Can I import machines created with other tools?**
JSON import is supported. XState v4/v5 machine definitions can often be imported with minor adjustments (on the roadmap: first-class XState import). DSL import is supported for FirmaState DSL files.

**Does FirmaState work offline?**
The current hosted preview requires an internet connection. A self-hosted / offline option is on the longer-term roadmap.

**Is the source code available?**
The source code is not yet published. Open-sourcing the runtime library is on the roadmap. The IDE itself will likely remain proprietary with a generous free tier.

---

## Glossary

| Term | Definition |
|---|---|
| **Action** | A pure function executed on transition entry, exit, or the transition itself. Updates context; produces no external effects. |
| **Authority model** | The system by which capabilities must be declared and granted before effects are executed. |
| **Canonical DSL** | The text representation of the machine model. Always in sync with the graph. The interchange format. |
| **Capability** | A named permission the machine declares it needs. Must be granted before the associated effects are executed. |
| **Compound state** | A state that contains child states. The machine occupies one child state at a time within a compound state. |
| **Context** | Typed data carried alongside the active state set. Updated by actions. |
| **Controlled simulation** | Execution of a machine where effects are intercepted and logged rather than executed, keeping the run side-effect free. |
| **Deterministic export** | A code-generation pass that produces identical output for identical input models, every time. |
| **Effect** | A described side effect bound to a capability. Executed by a runtime handler, not inline in the machine. |
| **Effect handler** | A function registered at runtime that executes a declared effect when the machine fires it. |
| **Event** | A named signal that drives state transitions. May carry a typed payload. |
| **Final state** | A state that, when entered, marks the machine (or its enclosing compound state) as complete. |
| **Guard** | A boolean condition evaluated against context and event payload to determine whether a transition fires. |
| **History node** | A pseudostate that, when targeted, restores the previously active substate. Shallow restores one level; deep restores the full active state tree. |
| **Machine model** | The canonical, in-memory representation of a state machine. The single source of truth in FirmaState. |
| **Parallel state** | A state whose child regions are all active simultaneously. |
| **Provenance block** | Metadata embedded in the exported TypeScript: model hash, export timestamp, tool version. |
| **Region** | A concurrently active subdivision of a parallel state. |
| **State** | A named node in the machine that represents a stable condition the system can occupy. |
| **Statechart** | A hierarchical, extended finite state machine formalism introduced by David Harel. FirmaState's machine model extends statecharts with a capability and effect layer. |
| **Transition** | A directed edge from a source state to a target state, triggered by an event, optionally guarded. |

---

## Contributing

FirmaState is not yet accepting code contributions — the codebase is not publicly available. Contributions of the following kinds are very welcome:

- **Bug reports and UX feedback** — use the preview at https://polish-pass-2.emergent.host and report issues to support@torafirma.com
- **Feature requests** — describe the machine authoring or export workflow you need
- **DSL feedback** — if the current DSL shape is unclear or missing constructs, open an issue or email the team
- **Template contributions** — if you have a well-designed machine pattern you'd like added to the template library, get in touch

When the runtime library is open-sourced (see roadmap), a contribution guide will be published in this repository.

---

## Contact

| Purpose | Address |
|---|---|
| Support and product feedback | support@torafirma.com |
| General enquiries | firmastate@torafirma.com |
| Commercial enquiries and partnerships | admin@torafirma.com |

Issues and discussions can also be opened in this GitHub repository.

---

## Current status

FirmaState is under active development. The current preview demonstrates the core machine model, graph/DSL synchronization, controlled simulation, capabilities and effects surface, and deterministic TypeScript export.

APIs, UX details, and runtime contracts may evolve before the first public release.

---

*FirmaState — make the machine the authority.*

