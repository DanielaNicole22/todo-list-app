# Quickstart Validation: Core Task Management

## Setup and automated gates

Requires Node.js 18 or newer and npm. From the repository root:

```powershell
npm ci
npm test -- --watchAll=false
npx tsc --noEmit
npm run build
npm start
```

Every non-server command must succeed. Open `http://localhost:3000` for manual validation.

## End-to-end scenarios

### Create, validate, and restore

1. Submit empty, whitespace-only, and over-500-character values; verify no task is created and the
   visible error is associated with and announced for the input.
2. Add two valid tasks (duplicate text is allowed); verify creation order and returned input focus.
3. Refresh; verify text, order, and state return.

### Complete, edit, and delete

1. Toggle completion using only the keyboard; verify state does not depend on color and persists.
2. Edit then press Escape; verify no change and focus on Edit.
3. Edit then press Enter; verify trimmed text, preserved completion, persistence, and focus on Edit.
4. Delete a middle, last, and final task; verify focus moves to next, previous, then input.

### Persistence failures

1. Simulate read/write failures in tests.
2. Verify the session remains usable and one accessible warning explains possible loss.
3. Restore storage, mutate again, and verify a successful full save clears the warning.

### Responsive and volume checks

1. Run the complete keyboard flow at 320px and 1440px.
2. Verify wrapping, visible controls/focus, and no unintended horizontal scrollbar.
3. Exercise 100 tasks; verify display and actions respond visibly within one second.

See [data-model.md](./data-model.md), [ui-contract.md](./contracts/ui-contract.md), and
[persistence-contract.md](./contracts/persistence-contract.md) for detailed expectations.
