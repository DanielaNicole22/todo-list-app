# Spec Kit Playbook for the Todo List App

This guide explains how to use the Spec Kit installation already present in this repository to develop the todo-list app. For installation and setup instructions, see [SPEC_KIT.md](./SPEC_KIT.md).

## Current project state

Spec Kit is configured for Codex with PowerShell scripts and skill-based commands. The React application is still an early scaffold: `App.js` renders an empty container and the files under `src/components/` are placeholders.

This makes the project a good candidate for incremental, specification-driven development. Build one coherent user-facing feature at a time instead of describing the entire future application in one large request.

## 1. Complete the project constitution

The constitution currently contains template placeholders. Complete it before specifying features so every plan and implementation follows the same standards.

In Codex, invoke:

```text
$speckit-constitution
```

Suggested prompt:

```text
Create a constitution for this React todo-list application with these
principles:

1. Build small, focused, reusable React components.
2. Test behavior that users can observe, including validation and persistence.
3. Meet keyboard navigation and accessible-label requirements.
4. Use responsive, consistent styling for mobile and desktop screens.
5. Keep the architecture simple and client-side unless an approved feature
   requires a backend.

Require npm test and npm run build to pass before a feature is complete.
```

Review the result in `.specify/memory/constitution.md`. Treat this file as the shared engineering agreement for the project.

## 2. Start with the core task-management feature

Invoke:

```text
$speckit-specify
```

Use a requirements-focused prompt:

```text
Create the core task-management experience for the todo-list app. A user can
add a task with non-empty text, view all tasks, mark a task complete or active,
edit its text, and delete it. Tasks persist after a browser refresh. Empty or
whitespace-only tasks are rejected with an accessible validation message. The
interface works with a keyboard and adapts to mobile and desktop widths.
```

Describe outcomes and user behavior during specification. Leave choices such as component boundaries, hook usage, and storage helpers for the planning stage.

Spec Kit will create a feature directory containing artifacts such as `spec.md`. Read the generated specification before proceeding and correct any requirement that does not reflect the intended product.

## 3. Resolve unclear requirements

Invoke:

```text
$speckit-clarify
```

Answer the targeted questions based on the simplest useful first release. Reasonable initial decisions for this app include:

- Store tasks in browser `localStorage`.
- Keep task text as the only required task field.
- Allow editing both active and completed tasks.
- Ask for confirmation before permanent deletion only if the specification requires it.
- Preserve the order in which tasks were added.

The clarification process writes decisions back into the specification so they are not lost between stages.

## 4. Generate the technical plan

Invoke:

```text
$speckit-plan
```

Provide project-specific technical context if requested:

```text
Use the existing React 18 and Create React App setup. Reuse the components in
src/components where appropriate. Use React state and localStorage without
adding a backend or state-management library. Use React Testing Library for
user-focused tests. Avoid adding dependencies unless the existing packages
cannot meet a stated requirement.
```

Review `plan.md` and any generated design artifacts. Confirm that the plan:

- Works with the dependencies already listed in `package.json`.
- Identifies how tasks are represented and persisted.
- Covers loading malformed or missing local data safely.
- Includes accessibility and responsive-design work.
- Includes automated tests.

## 5. Turn the plan into tasks

Invoke:

```text
$speckit-tasks
```

The resulting `tasks.md` should be ordered so foundational work precedes dependent UI work. A sensible sequence for the first feature is:

1. Define the task data shape and persistence helper.
2. Implement task state and actions in the wrapper component.
3. Implement creation and validation in the form.
4. Render individual tasks with completion, edit, and delete actions.
5. Connect the components through `App.js`.
6. Add accessible and responsive styling.
7. Add behavior and persistence tests.
8. Run the test and production-build checks.

If the generated tasks omit a stated requirement, revise the artifacts before implementation.

## 6. Check artifact consistency

Invoke:

```text
$speckit-analyze
```

This checks `spec.md`, `plan.md`, and `tasks.md` without changing source code. Resolve important findings such as:

- A requirement with no implementation task.
- A planned component that does not support a user story.
- Tests that fail to cover acceptance criteria.
- Conflicting persistence or validation decisions.

Optionally invoke `$speckit-checklist` to create a focused requirements checklist. Useful requests include:

```text
Create an accessibility requirements checklist for the task-management feature.
```

```text
Create a persistence and data-recovery requirements checklist for localStorage.
```

## 7. Implement the approved tasks

Before implementation, commit or otherwise checkpoint the current repository state. Then invoke:

```text
$speckit-implement
```

The implementation skill processes the work recorded in `tasks.md`. Review its changes as you would review a normal code contribution. Do not treat generated code as correct solely because it came from an approved specification.

After implementation, run:

```powershell
npm test -- --watchAll=false
npm run build
```

For manual verification, run:

```powershell
npm start
```

Open [http://localhost:3000](http://localhost:3000) and exercise every acceptance scenario from the specification.

If the implementation does not yet satisfy all generated artifacts, invoke:

```text
$speckit-converge
```

This compares the implementation with the specification, plan, and tasks, then appends remaining work to `tasks.md`. Run `$speckit-implement` again to complete those tasks.

## 8. Develop later features separately

Once the core task-management feature is complete, create a new specification for each substantial capability. Good candidates include:

### Task filtering

```text
$speckit-specify

Add All, Active, and Completed filters. The selected filter is visually and
programmatically identifiable, keyboard accessible, and does not change the
stored tasks.
```

### Due dates and priorities

```text
$speckit-specify

Allow optional due dates and Low, Medium, or High priorities. Clearly identify
overdue incomplete tasks and define predictable sorting behavior.
```

### Search

```text
$speckit-specify

Allow users to search tasks by text with immediate, case-insensitive results.
Searching must work together with the selected task-status filter.
```

### Bulk actions

```text
$speckit-specify

Allow users to mark all visible tasks complete and clear completed tasks while
protecting users from accidental destructive actions.
```

### Categories

```text
$speckit-specify

Allow users to assign an optional category to a task, create categories, and
filter tasks by category. Existing uncategorized tasks must continue to work.
```

Run the clarify, plan, tasks, analyze, and implement stages independently for each feature. This keeps requirements reviewable and prevents unrelated changes from becoming coupled.

## Working habits that make Spec Kit useful

- Keep one feature focused on one coherent user outcome.
- Review every generated Markdown artifact before moving to the next stage.
- Put product behavior in `spec.md` and technical decisions in `plan.md`.
- Record missing implementation work in `tasks.md` instead of relying on chat history.
- Ask for clarification before implementation when a decision affects stored user data or destructive behavior.
- Use acceptance scenarios as the basis for automated and manual tests.
- Commit feature artifacts together with the source code so future contributors can understand why the implementation exists.
- Start a new specification when scope changes substantially instead of silently expanding an existing feature.

## Recommended workflow summary

```text
$speckit-constitution       # Once initially, then only when principles change

$speckit-specify           # Start each feature
$speckit-clarify           # Resolve ambiguity
$speckit-plan              # Choose the technical approach
$speckit-tasks             # Create ordered implementation work
$speckit-analyze           # Check consistency
$speckit-implement         # Modify and test the source code
$speckit-converge          # Find remaining work when needed
```

Spec Kit is most valuable when its artifacts are reviewed decisions, not merely generated paperwork. The specification defines success, the plan explains the approach, the tasks make the work traceable, and the tests demonstrate that the implemented feature meets the agreed behavior.
