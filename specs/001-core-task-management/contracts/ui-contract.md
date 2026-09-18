# UI Contract: Core Task Management

## App

Renders a visible page heading and one `TodoWrapper`; owns no task state.

## TodoWrapper

- Owns tasks, active edit, persistence status, mutations, and focus coordination.
- Loads once and attempts a complete save after each accepted mutation.
- Renders an empty message or a semantic task list in creation order.
- Shows one promptly announced, non-blocking persistence warning on failure.

### Focus outcomes

- Add -> new-task input.
- Save/cancel -> edited task's Edit button.
- Delete -> next task's first control, otherwise previous task's first control, otherwise input.

Focus moves after the target exists in the updated interface.

## TodoForm

- Provides a visibly labeled input and native submit control.
- Does not silently truncate pasted text; submission validates trimmed length.
- Invalid submit preserves the draft and shows an associated, announced error.
- Successful submit clears draft/error, adds one task, and focuses the input.

## Todo

- Is a semantic list item with one immutable task value.
- Uses a semantic binary control for completion and conveys state without color alone.
- Uses native Edit/Delete buttons whose accessible names identify the affected task.
- Replaces display actions with `EditTodoForm` while editing.

## EditTodoForm

- Provides a labeled edit input plus visible Save and Cancel buttons.
- Normal form submission makes Enter save; Escape cancels through scoped key handling.
- Invalid save retains the draft and displays associated, announced feedback.
- Successful save trims text, preserves completion, exits edit mode, and focuses Edit.
- Cancel discards the draft, exits edit mode, and focuses Edit.

## Responsive and announcement contract

- From 320px through 1440px, no unintended horizontal scrolling occurs.
- Long text and action groups wrap without changing logical source/tab order.
- All controls have visible `:focus-visible` indication.
- Validation and persistence messages are visible and exposed with live/error semantics.
- Unchanged messages are not repeatedly announced on ordinary rerenders.
