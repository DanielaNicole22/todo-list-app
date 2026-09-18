# Data Model: Core Task Management

## Task

| Field | Type | Required | Rules |
|---|---|---:|---|
| `id` | string | Yes | Non-empty, stable, and unique in the collection. Generated once at creation. |
| `text` | string | Yes | Trimmed; 1–500 characters. Duplicate text is allowed. |
| `completed` | boolean | Yes | `false` initially; changes independently of text. |

### Invariants

- Each ID occurs at most once; actions select tasks by ID rather than text or position.
- Editing changes only text; toggling changes only completion; deletion mutates no other task.
- Collection order is creation order and does not change during edit or toggle.

### State transitions

```text
create(valid text) -> active
active --toggle--> completed
completed --toggle--> active
active|completed --edit(valid text)--> same completion state
active|completed --delete--> removed
edit --cancel--> original task unchanged
```

Invalid create/edit text produces feedback and no task mutation.

## Persisted Task Document

| Field | Type | Required | Rules |
|---|---|---:|---|
| `version` | number | Yes | Exactly `1`; other values are unsupported. |
| `tasks` | Task array | Yes | Every task is valid and IDs are unique. |

Unknown stored input is validated completely. Invalid JSON, version, shape, field, or duplicate ID
produces an empty session collection and persistence warning. Invalid input is not overwritten on load.

## Task Collection

- The current-session source of truth, initialized from valid storage or empty.
- Applies changes immediately in creation order and attempts a complete save after every mutation.
- Remains usable on storage failure.
- Excludes form drafts, errors, edit state, and persistence status.

## Transient UI State

- New-task draft and optional error.
- Edited task ID, edit draft, and optional error; only one edit is active.
- Persistence status: `available` or `unavailable`.
- Pending focus target: new-task input, a task's first control, or a task's Edit control.

A later successful complete save changes persistence status back to `available` and clears its warning.

## Validation Feedback

| Condition | Result |
|---|---|
| Trimmed text is empty | Reject and state that task text is required. |
| Trimmed text exceeds 500 characters | Reject and state the 500-character limit. |
| Trimmed text is 1–500 characters | Accept and clear obsolete feedback. |

Feedback is associated with the relevant input, announced, and never persisted.
