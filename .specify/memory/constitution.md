<!--
Sync Impact Report
- Version change: template -> 1.0.0
- Modified principles:
  - Template Principle 1 -> I. Focused, Reusable Components
  - Template Principle 2 -> II. Observable-Behavior Testing
  - Template Principle 3 -> III. Accessible by Default
  - Template Principle 4 -> IV. Responsive, Consistent Experience
  - Template Principle 5 -> V. Client-Side Simplicity
- Added sections:
  - Technical Constraints
  - Development Workflow and Quality Gates
- Removed sections: none
- Follow-up TODOs: none
-->
# Todo List App Constitution

## Core Principles

### I. Focused, Reusable Components

React components MUST have one clear responsibility and a minimal, explicit interface.
Shared behavior or presentation MUST be extracted only when it has a demonstrated reuse case or
when extraction materially improves testability. Components MUST remain independently testable,
and feature-specific behavior MUST stay close to the feature that owns it. Large components MUST
be decomposed when they mix unrelated state, behavior, or presentation. This keeps changes local,
reduces coupling, and makes the interface easier to understand.

### II. Observable-Behavior Testing

Automated tests MUST verify behavior visible to a user rather than internal implementation details.
Every feature MUST test its acceptance scenarios, including success paths, validation failures, and
state changes. Any feature that persists data MUST test saving, restoring, and safely handling
missing or invalid stored data. Tests MUST use accessible queries wherever practical and MUST remain
valid when internal component structure changes without changing behavior.

### III. Accessible by Default

Every interactive workflow MUST be operable with a keyboard alone. Controls MUST use semantic HTML
where possible and MUST have an accessible name that describes their action. Focus order and focus
visibility MUST remain usable, form errors and status changes MUST be available to assistive
technology, and visual state MUST NOT be conveyed by color alone. Accessibility requirements MUST
be included in feature acceptance scenarios and verified before completion.

### IV. Responsive, Consistent Experience

The interface MUST remain usable at mobile and desktop viewport widths without obscured controls,
unreadable text, or unintended horizontal scrolling. New UI MUST reuse the application's established
spacing, typography, color, control, and interaction patterns. Responsive behavior MUST be designed
as part of the feature rather than added after implementation, and materially different layouts MUST
be checked at representative narrow and wide viewport sizes.

### V. Client-Side Simplicity

The application MUST remain a simple client-side React and TypeScript application by default.
Features MUST use the existing platform and dependencies when they satisfy the requirements. A new
dependency, architectural layer, remote service, or backend MUST have an approved feature requirement
and a documented justification showing why a simpler client-side solution is insufficient. Designs
MUST avoid speculative abstractions and infrastructure for unrequested future needs.

## Technical Constraints

- Application code MUST use React and TypeScript and MUST preserve strict type checking.
- Persistent browser data MUST have an explicit schema or type and MUST be parsed defensively.
- Secrets and privileged operations MUST NOT be placed in client-side code.
- Dependencies MUST be necessary for an approved requirement, compatible with the current toolchain,
  and recorded in both `package.json` and `package-lock.json`.
- Source changes MUST avoid unrelated refactoring unless it is required to deliver or safely test the
  approved feature.

## Development Workflow and Quality Gates

Each feature MUST begin with testable acceptance scenarios. The specification and implementation plan
MUST identify component responsibilities, user-visible validation, persistence behavior when relevant,
keyboard behavior, accessible names, and responsive expectations before implementation begins.

Code review MUST verify compliance with every applicable principle. Before a feature is complete:

1. All acceptance scenarios MUST be implemented and reviewed.
2. `npm test` MUST pass; a non-interactive equivalent such as
   `npm test -- --watchAll=false` MAY be used in automation.
3. `npm run build` MUST pass without compilation errors.
4. Keyboard navigation and representative mobile and desktop layouts MUST be manually checked when
   the feature changes interactive UI.
5. Any failure or intentionally deferred requirement MUST be documented and prevents the feature from
   being marked complete until explicitly approved as a scope change.

## Governance

This constitution governs all specifications, plans, tasks, implementations, and reviews in this
repository. When another project document or customary practice conflicts with it, this constitution
takes precedence.

Amendments MUST be proposed as an explicit constitution change, explain the reason and migration
impact, update the Sync Impact Report, and receive project-owner approval before dependent work is
treated as compliant. Versions follow semantic versioning: MAJOR for incompatible principle removals
or redefinitions, MINOR for new principles or materially expanded obligations, and PATCH for
clarifications that do not change obligations. The ratification date remains the original adoption
date; the last-amended date changes whenever governance content changes.

Every feature review MUST confirm applicable principle compliance and completion of the quality gates.
Exceptions MUST be documented with their scope, rationale, approver, and remediation plan; an
undocumented exception is non-compliant.

**Version**: 1.0.0 | **Ratified**: 2026-09-18 | **Last Amended**: 2026-09-18
