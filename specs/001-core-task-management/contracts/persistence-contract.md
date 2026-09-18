# Persistence Contract: Core Task Management

## Boundary and key

Only the persistence module directly accesses browser storage. It uses:

```text
todo-list-app.tasks
```

The value is the versioned document from [data-model.md](../data-model.md).

## Load

Return validated `tasks` plus `persistenceAvailable`.

1. Read once during initialization.
2. A missing key is a successful empty state.
3. Parse inside an error boundary.
4. Validate version, array, every field, text rules, and unique IDs.
5. Preserve stored order when all checks pass.
6. On access/parse/schema failure, return empty tasks and unavailable status without throwing.
7. Do not overwrite invalid content merely because loading failed.

## Save

Input is the complete task collection after one accepted mutation.

1. Construct and serialize one complete versioned snapshot.
2. Write after add, toggle, valid edit, or delete.
3. Do not write after rejected validation or canceled editing.
4. Report failure without throwing into the UI or rolling back session state.
5. Retry a complete save on the next accepted mutation; success clears the warning.

The UI receives status, not raw exceptions, and displays a stable message equivalent to:

> Changes are available for this session but may be lost when you refresh or close the page.
