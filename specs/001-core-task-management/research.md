# Research: Core Task Management

## State ownership

**Decision**: Keep the ordered `Task[]` and mutations in `TodoWrapper`; pass typed values and callbacks
to the focused child components.

**Rationale**: One owner supplies a clear source of truth for actions, persistence, and focus while
reusing the existing component shells.

**Alternatives considered**: Context/reducer or an external store adds unnecessary structure; row-owned
task state complicates complete persistence and coordination.

## Persistence format and boundary

**Decision**: Use a typed adapter with one application key and `{ version: 1, tasks: Task[] }`; validate
unknown parsed values field by field.

**Rationale**: The version supports future migration, while runtime validation prevents malformed data
from entering strict application state.

**Alternatives considered**: A raw array is less evolvable; type assertions do not validate runtime
input; a backend or IndexedDB is outside scope.

## Load, save, and failure handling

**Decision**: Load once through a lazy initializer/helper and explicitly save a complete snapshot after
each accepted mutation. Catch read/write errors, keep session state, and expose typed status.

**Rationale**: Explicit mutation-time saving avoids a mount effect overwriting unreadable data and makes
failure/recovery behavior directly testable.

**Alternatives considered**: Direct storage calls can break rendering; save-only effects have initial
write and StrictMode hazards; blocking changes contradicts the clarified behavior.

## Text validation

**Decision**: Share one pure normalization function: trim, require 1–500 characters, and return a
field-specific error.

**Rationale**: Add and edit obey identical rules and invalid text never mutates a task.

**Alternatives considered**: HTML attributes alone do not consistently reject whitespace or provide
the required messages; duplicated validation can drift.

## Semantics and announcements

**Decision**: Use labeled forms and inputs, a semantic completion control, native named buttons, and
visible errors linked using `aria-invalid`/`aria-describedby` with appropriate live/error semantics.

**Rationale**: Native semantics provide keyboard support with minimal code and task-qualified names
disambiguate repeated controls.

**Alternatives considered**: Clickable non-controls require fragile keyboard emulation; unlabeled
icon-only controls violate the specification.

## Editing and focus

**Decision**: Use an inline edit form; form submission handles Enter and scoped key handling handles
Escape. Coordinate post-render focus from `TodoWrapper` with the input ref and task-control registry.

**Rationale**: State transitions replace DOM nodes, so a pending focus target applied after commit is
deterministic for duplicate text, save/cancel, and neighbor selection after deletion.

**Alternatives considered**: Global listeners can conflict; timers and DOM queries are brittle; no
focus management violates clarified outcomes.

## Responsive styling

**Decision**: Replace demo CSS with a mobile-first bounded container, `min-width: 0`, wrapping text and
actions, full-width inputs where useful, explicit `:focus-visible`, and a desktop enhancement breakpoint.

**Rationale**: CSS alone meets 320–1440px requirements and safely contains 500-character text.

**Alternatives considered**: Fixed, non-wrapping rows overflow; a UI framework is unjustified.

## Testing

**Decision**: Use existing Jest/React Testing Library with a jest-dom setup file, accessible queries,
`userEvent`, mocked storage failures, and remounts for persistence.

**Rationale**: This tests observable behavior without adding dependencies. Layout overflow remains a
manual viewport check because jsdom does not calculate real layout.

**Alternatives considered**: Snapshots couple tests to markup; Enzyme/browser automation adds packages;
jsdom timing/layout claims would not credibly validate paint performance or overflow.

## Stable identity

**Decision**: Use the existing UUID dependency for task identifiers.

**Rationale**: Stable identity supports isolated mutation, React keys, persistence, and focus targeting.

**Alternatives considered**: Indexes and timestamps risk collisions or incorrect targeting; another ID
package is unnecessary.
