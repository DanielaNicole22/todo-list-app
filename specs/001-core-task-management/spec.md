# Feature Specification: Core Task Management

**Feature Branch**: `not-created`

**Created**: 2026-09-18

**Status**: Draft

**Input**: User description: "Create the core task-management experience for the todo-list app. A user can add a task with non-empty text, view all tasks, mark a task complete or active, edit its text, and delete it. Tasks persist after a browser refresh. Empty or whitespace-only tasks are rejected with an accessible validation message. The interface works with a keyboard and adapts to mobile and desktop widths."

## Clarifications

### Session 2026-09-18

- Q: What maximum length should be allowed for task text? -> A: 500 characters.
- Q: If task persistence is unavailable or saving fails, how should the application behave? -> A:
  Continue for the current session and show an accessible persistence warning.
- Q: While editing a task, which keyboard shortcuts should save or cancel the edit? -> A: Enter
  saves and Escape cancels; visible Save and Cancel controls remain available.
- Q: Where should keyboard focus move after adding, saving, canceling, or deleting a task? -> A:
  Move focus contextually to the nearest useful control.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Capture and Review Tasks (Priority: P1)

A user adds a task and immediately sees it in the task list so they can record and review work that
needs attention. The task remains available after the page is refreshed.

**Why this priority**: Creating, viewing, and retaining tasks provides the minimum useful todo-list
experience. Without this journey, the other task actions have no lasting value.

**Independent Test**: Starting with no tasks, add a valid task, confirm that it appears in the list,
refresh the page, and confirm that the same task is still visible and active.

**Acceptance Scenarios**:

1. **Given** the task list is empty, **When** the user submits non-empty task text, **Then** one active
   task with that text appears in the list.
2. **Given** one or more tasks exist, **When** the user views the list, **Then** every saved task and
   its current completion state are visible.
3. **Given** saved tasks exist, **When** the user refreshes or reopens the page in the same browser,
   **Then** the saved tasks reappear with their text and completion states unchanged.
4. **Given** the task entry contains only whitespace, **When** the user submits it, **Then** no task is
   created and a visible, assistive-technology-readable validation message explains that task text is
   required.
5. **Given** a validation message is displayed, **When** the user supplies valid task text and submits
   it, **Then** the task is created and the obsolete validation message is cleared.
6. **Given** the task entry exceeds 500 characters, **When** the user attempts to submit it, **Then**
   no task is created and an accessible validation message explains the 500-character limit.
7. **Given** persistence is unavailable or a save fails, **When** the user changes tasks, **Then** the
   change remains usable for the current session and an accessible warning explains that it may not
   survive a refresh.
8. **Given** a task is successfully added, **When** the interface finishes updating, **Then** keyboard
   focus returns to the new-task input.

---

### User Story 2 - Track Completion (Priority: P2)

A user marks a task complete when the work is done and can return it to active when more work is
needed, while retaining the task in the list.

**Why this priority**: Completion state turns a static list into a useful progress tracker and is the
most important action after capturing tasks.

**Independent Test**: With one active task present, mark it complete, confirm that its state is clear,
mark it active again, and refresh after each state change to confirm that the latest state remains.

**Acceptance Scenarios**:

1. **Given** an active task exists, **When** the user marks it complete, **Then** the task remains in
   the list and is clearly identified as complete without relying on color alone.
2. **Given** a completed task exists, **When** the user marks it active, **Then** the task remains in
   the list and is clearly identified as active.
3. **Given** the user changes a task's completion state, **When** the page is refreshed, **Then** the
   most recently selected state remains.

---

### User Story 3 - Correct and Remove Tasks (Priority: P3)

A user edits task text to correct or refine it and deletes a task that is no longer needed.

**Why this priority**: Editing and deletion keep the list accurate, but users can receive the core
value of recording and tracking tasks before these maintenance actions are available.

**Independent Test**: With a saved task present, change its text and confirm the revision persists;
then delete it and confirm that it does not return after a refresh.

**Acceptance Scenarios**:

1. **Given** a task exists, **When** the user begins editing it, changes the text to a non-empty value,
   and saves, **Then** the revised text replaces the previous text without changing completion state.
2. **Given** a task is being edited, **When** the user attempts to save empty or whitespace-only text,
   **Then** the original task remains unchanged and an accessible validation message explains that
   task text is required.
3. **Given** a task is being edited, **When** the user attempts to save text exceeding 500 characters,
   **Then** the original task remains unchanged and an accessible validation message explains the
   500-character limit.
4. **Given** a task exists, **When** the user deletes it, **Then** it is removed from the visible list
   and does not return after a page refresh.
5. **Given** a task is being edited, **When** the user cancels editing, **Then** the original task text
   and completion state remain unchanged.
6. **Given** a task is being edited with valid text, **When** the user presses Enter, **Then** the edit
   is saved with the same result as activating the visible Save control.
7. **Given** a task is being edited, **When** the user presses Escape, **Then** the edit is canceled
   with the same result as activating the visible Cancel control.
8. **Given** a task edit is saved or canceled, **When** the interface returns to display mode, **Then**
   focus moves to the edited task's Edit control.
9. **Given** a focused task is deleted, **When** another task follows it, **Then** focus moves to that
   next task's first control.
10. **Given** a focused task is deleted and no task follows it, **When** another task precedes it,
    **Then** focus moves to that previous task's first control.
11. **Given** the final task is deleted, **When** the list becomes empty, **Then** focus moves to the
    new-task input.

---

### User Story 4 - Operate Across Devices and Input Methods (Priority: P4)

A user completes every task-management action using a keyboard and can use the interface at typical
mobile and desktop viewport widths.

**Why this priority**: The core experience must remain available regardless of input method or screen
size; this story verifies those requirements across the other user journeys.

**Independent Test**: Complete creation, completion toggling, editing, canceling, saving, and deletion
using only the keyboard at narrow and wide viewport sizes, confirming visible focus and usable layout.

**Acceptance Scenarios**:

1. **Given** the application is open, **When** the user navigates with only the keyboard, **Then** every
   interactive control receives visible focus in a logical order and can be activated.
2. **Given** an interactive task control is present, **When** it receives focus, **Then** its accessible
   name identifies both its action and the associated task where needed for clarity.
3. **Given** the interface is displayed at a representative mobile width, **When** the user performs
   each task action, **Then** content remains readable and controls remain visible and operable without
   unintended horizontal scrolling.
4. **Given** the interface is displayed at a representative desktop width, **When** the user performs
   each task action, **Then** the layout remains consistent, readable, and operable.

### Edge Cases

- Leading and trailing whitespace is removed from submitted task text; text containing non-whitespace
  characters remains valid.
- Repeated task text is allowed because separate tasks may intentionally have the same description.
- Adding, editing, toggling, or deleting one task does not alter any other task.
- A task keeps its completion state while its text is edited.
- Canceling an edit after changing the field discards the unsaved changes.
- An unavailable or unreadable saved task collection results in an empty usable list rather than a
  broken interface; the user can still manage tasks for the current session and receives an accessible
  warning that changes may not survive a refresh.
- A long task description wraps within the available width without hiding task actions or creating
  unintended horizontal scrolling.
- Rapid repeated activation of a task action produces one consistent final state and does not create
  unintended duplicate tasks.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The product MUST allow a user to create a task by submitting text containing at least
  one non-whitespace character and no more than 500 characters after trimming.
- **FR-002**: The product MUST trim leading and trailing whitespace before saving task text.
- **FR-003**: The product MUST reject empty or whitespace-only text during task creation and editing
  and MUST reject text exceeding 500 characters without creating or overwriting a task.
- **FR-004**: Rejected text MUST produce a visible validation message that is announced to assistive
  technology and associated with the relevant input.
- **FR-005**: The product MUST display every saved task with its text and current active or completed
  state.
- **FR-006**: The product MUST allow a user to change an active task to completed and a completed task
  back to active.
- **FR-007**: Completion state MUST be identifiable without relying on color alone.
- **FR-008**: The product MUST allow a user to edit a task's text while preserving that task's
  completion state.
- **FR-009**: The product MUST allow a user to cancel an edit without changing the saved task.
- **FR-009A**: While editing, pressing Enter MUST save valid task text and pressing Escape MUST cancel
  the edit; visible Save and Cancel controls MUST remain available.
- **FR-010**: The product MUST allow a user to permanently delete an individual task.
- **FR-011**: Creation, text edits, completion changes, and deletions MUST remain in effect after a
  page refresh or later visit from the same browser and device.
- **FR-012**: If saved task data cannot be read as a valid task collection, the product MUST remain
  usable, present an empty list, and allow tasks to be managed for the current session.
- **FR-013**: Every action MUST affect only the selected task, except creation which adds one task.
- **FR-014**: All task-management actions MUST be operable using only a keyboard.
- **FR-015**: Every interactive control MUST have a clear accessible name; task-specific controls MUST
  distinguish the task they affect when context would otherwise be ambiguous.
- **FR-016**: Keyboard focus MUST be visible and move through controls in a logical order.
- **FR-016A**: After adding a task, focus MUST return to the new-task input. After saving or canceling
  an edit, focus MUST move to that task's Edit control. After deleting a task, focus MUST move to the
  next task's first control, otherwise the previous task's first control, or the new-task input when
  no tasks remain.
- **FR-017**: The interface MUST keep content readable and controls visible and operable at
  representative mobile and desktop viewport widths without unintended horizontal scrolling.
- **FR-018**: The product MUST allow multiple tasks to have identical text while maintaining them as
  independently editable, completable, and deletable tasks.
- **FR-019**: If persistence is unavailable or a save fails, the product MUST preserve task changes
  for the current session and MUST display a visible warning announced to assistive technology that
  the changes may not survive a refresh.

### Key Entities

- **Task**: A distinct item of work with a stable identity, user-provided text, and an active or
  completed state.
- **Task Collection**: The ordered set of saved tasks shown to the user and restored on later visits.
- **Validation Feedback**: A message tied to a task text entry that explains why the submitted value
  was rejected and is perceivable visually and by assistive technology.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: In usability verification, at least 90% of first-time users can add a task, mark it
  complete, edit it, and delete it without assistance.
- **SC-002**: A user can create a valid task and see it in the list within 10 seconds under normal use.
- **SC-003**: In 100% of acceptance tests, valid task changes remain accurate after a page refresh,
  including task text, completion state, additions, and deletions.
- **SC-004**: In 100% of validation tests, empty or whitespace-only submissions create no task or
  overwrite; the same is true for submissions exceeding 500 characters. Every rejection produces
  feedback perceivable both visually and by assistive technology.
- **SC-005**: Every task-management action can be completed without a pointing device, with visible
  focus throughout the workflow.
- **SC-006**: At representative viewport widths from 320 through 1440 CSS pixels, all content remains
  readable and all controls remain usable without unintended horizontal scrolling.
- **SC-007**: A collection of at least 100 tasks remains fully viewable and supports creation,
  completion changes, editing, and deletion without user-visible delays longer than one second.
- **SC-008**: In 100% of simulated persistence failures, task management remains usable for the
  current session and the user receives a visible, assistive-technology-readable warning before a
  refresh can discard changes.

## Assumptions

- The application serves one local user; accounts, sign-in, sharing, and cross-device synchronization
  are outside this feature's scope.
- Tasks contain text and completion state only; due dates, priorities, categories, search, filtering,
  sorting controls, and bulk actions are outside this feature's scope.
- Tasks are displayed in creation order unless a later feature defines alternative ordering.
- Deletion takes effect immediately without a confirmation step or undo capability.
- Persistence is required only for later visits using the same browser and device.
- The user has access to a browser that supports the application's standard interactive and
  persistence capabilities.
