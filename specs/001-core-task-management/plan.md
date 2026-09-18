# Implementation Plan: Core Task Management

**Branch**: `001-core-task-management` | **Date**: 2026-09-18 | **Spec**: [spec.md](./spec.md)

**Input**: Feature specification from `specs/001-core-task-management/spec.md`

## Summary

Build the complete todo workflow in the existing React 18 and TypeScript application. A single
container owns the ordered tasks, edit state, persistence status, and actions. Focused form and task
components expose typed props and semantic controls. A small storage module validates a versioned
local task document, saves accepted changes, and reports failures without blocking the session.
User-focused tests cover behavior, persistence, accessibility, keyboard use, and focus restoration.

## Technical Context

**Language/Version**: TypeScript 4.9.5 with strict checking; JSX through TSX

**Primary Dependencies**: React 18.2, React DOM 18.2, React Scripts 5.0.1, UUID 9.0; no new runtime
dependency planned

**Storage**: Browser `localStorage` using one versioned JSON document; in-memory React state remains
authoritative for the current session when reads or writes fail

**Testing**: Jest through React Scripts, React Testing Library 13.4, jest-dom 5.17, user-event 13.5

**Target Platform**: Browsers supported by the existing Browserslist configuration; layouts validated
from 320 through 1440 CSS pixels

**Project Type**: Single client-side web application

**Performance Goals**: With 100 tasks, initial display and each task action produce a visible result
in under one second

**Constraints**: No backend or state-management library; no dependency addition unless required;
trimmed task text is limited to 500 characters; same-browser/device persistence; usable on storage
failure

**Scale/Scope**: One local user, one ordered collection of at least 100 tasks, one application screen;
accounts, synchronization, filters, sorting controls, dates, priorities, and bulk actions are excluded

## Constitution Check

*GATE: Passed before Phase 0 research and passed again after Phase 1 design.*

| Constitutional requirement | Plan evidence | Status |
|---|---|---|
| Focused components | `TodoWrapper`, `TodoForm`, `Todo`, and `EditTodoForm` each have one typed responsibility. | PASS |
| Observable-behavior testing | Tests use roles, names, visible messages, persistence outcomes, and focus assertions. | PASS |
| Accessible by default | Semantic controls, associated errors, live status, visible focus, shortcuts, and focus restoration are in the UI contract. | PASS |
| Responsive consistency | Mobile-first layout and wrapping rows are checked at 320px and 1440px. | PASS |
| Client-side simplicity | React state and one localStorage boundary suffice; no backend, state library, or new package is added. | PASS |
| Strict types and defensive persistence | Explicit task/document types and runtime guards validate unknown saved input. | PASS |
| Quality gates | Quickstart requires `npm test`, type checking, `npm run build`, keyboard QA, and viewport QA. | PASS |

Post-design review: The model, contracts, and validation guide preserve every gate. No constitutional
exception or complexity justification is required.

## Project Structure

### Documentation (this feature)

```text
specs/001-core-task-management/
|-- plan.md
|-- research.md
|-- data-model.md
|-- quickstart.md
|-- contracts/
|   |-- persistence-contract.md
|   `-- ui-contract.md
|-- checklists/requirements.md
`-- tasks.md                    # Created later by $speckit-tasks
```

### Source Code (repository root)

```text
src/
|-- components/
|   |-- EditTodoForm.tsx        # Edit/cancel form and shortcuts
|   |-- Todo.tsx                # One task's display and actions
|   |-- TodoForm.tsx            # New-task entry and validation
|   `-- TodoWrapper.tsx         # State, actions, status, focus coordination
|-- models/task.ts              # Task/document types and guards
|-- services/taskStorage.ts     # Defensive localStorage boundary
|-- App.tsx                     # Shell rendering TodoWrapper
|-- App.css                     # Responsive system and focus states
|-- App.test.tsx                # End-to-end component behavior tests
|-- setupTests.ts               # jest-dom setup
`-- index.tsx                   # Existing entry point
```

**Structure Decision**: Keep the existing single Create React App layout. Reuse all component
placeholders, add only a domain model and persistence boundary, and test user flows at application
level so internal composition can change without invalidating tests.

## Phase 0: Research Decisions

[research.md](./research.md) resolves state ownership, validation, identity, persistence, accessible
messages, edit shortcuts, focus restoration, responsive styling, and testing without new dependencies.

## Phase 1: Design

- [data-model.md](./data-model.md) defines identity, validation, order, persisted schema, and states.
- [ui-contract.md](./contracts/ui-contract.md) defines responsibilities, semantics, events, and focus.
- [persistence-contract.md](./contracts/persistence-contract.md) defines storage and failures.
- [quickstart.md](./quickstart.md) defines automated and manual validation.

## Complexity Tracking

No constitution violations require justification.
