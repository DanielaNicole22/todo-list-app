# Using GitHub Spec Kit

GitHub Spec Kit adds a specification-driven development workflow to this repository. It does not directly replace or modify the React source code during setup. Instead, it creates project guidance and feature artifacts that Codex can use to plan and implement changes.

## Prerequisites

The application already requires Git and Node.js. Spec Kit additionally requires `uv`, which installs and runs the Specify CLI.

Install the following software:

1. Git
2. Node.js 18 or newer, including npm
3. [uv](https://docs.astral.sh/uv/getting-started/installation/)
4. Codex

After installing `uv`, restart PowerShell and confirm that the required commands are available:

```powershell
uv --version
git --version
node --version
npm --version
```

### PowerShell setup and troubleshooting

If `uv` is not installed, install it from PowerShell with WinGet:

```powershell
winget install --id=astral-sh.uv -e --accept-package-agreements --accept-source-agreements
```

Restart PowerShell after installation so the updated `PATH` is loaded. If you
cannot restart the current session, locate the executable and invoke it by its
full path:

```powershell
Get-ChildItem "$env:LOCALAPPDATA\Microsoft\WinGet" -Recurse -Filter uv.exe
```

The current Specify CLI requires Python 3.11 or newer. The machine's existing
Python 3.10 installation therefore cannot install current releases with
`python -m pip install --user specify-cli`. Using `uv` avoids changing that
Python installation because it provisions a compatible isolated runtime.

If `specify` is not installed or is not yet available on `PATH`, initialize the
project directly through `uv`:

```powershell
uv tool run --from git+https://github.com/github/spec-kit.git specify init --here --force --integration codex --integration-options="--skills" --script ps
```

In a PowerShell session whose `PATH` has not refreshed, replace `uv` in that
command with the full path returned by `Get-ChildItem` and invoke it with the
call operator (`&`), for example:

```powershell
& "C:\path\to\uv.exe" tool run --from git+https://github.com/github/spec-kit.git specify init --here --force --integration codex --integration-options="--skills" --script ps
```

## Install the Specify CLI

Find the newest version on the [official Spec Kit releases page](https://github.com/github/spec-kit/releases). Replace `vX.Y.Z` below with that release tag:

```powershell
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@vX.Y.Z
```

Verify the installation and check whether a newer version is available:

```powershell
specify version
specify self check
```

Use only the package distributed through the official GitHub Spec Kit repository.

## Initialize Spec Kit in this project

Commit or back up current changes before initialization. From the root of this repository, run:

```powershell
cd C:\Users\Daniela\Documents\personal\todo-list-app

specify init --here --force --integration codex --integration-options="--skills" --script ps
```

The options mean:

- `--here` initializes the current repository.
- `--force` permits initialization in an existing, non-empty directory.
- `--integration codex` configures Spec Kit for Codex.
- `--integration-options="--skills"` installs Codex-compatible skills.
- `--script ps` installs the PowerShell variants of the automation scripts.

Initialization should add Spec Kit files such as:

```text
.specify/
.agents/skills/
```

The existing application files under `src/` remain in place. Restart Codex or reopen the repository after initialization so it discovers the installed skills.

## Establish project principles

Invoke the constitution skill in Codex:

```text
$speckit-constitution
```

Suggested request:

```text
Create governing principles for this React todo-list app. Require small
reusable components, accessible controls, responsive design, automated
tests for user behavior, clear naming, and no backend unless a feature
explicitly requires one.
```

The constitution supplies project-wide rules that guide subsequent specifications, plans, and implementations.

## Specify the first feature

Invoke:

```text
$speckit-specify
```

Suggested specification request:

```text
Build the core todo-list experience. Users must be able to add a non-empty
task, see all tasks, mark a task complete, edit its text, and delete it.
Tasks should remain available after refreshing the browser. Show validation
when an empty task is submitted. The interface must work with keyboard
navigation and on mobile screens.
```

At this stage, concentrate on the required user experience and outcomes instead of React implementation details.

## Run the feature workflow

After creating the feature specification, invoke the skills in this order:

```text
$speckit-clarify
$speckit-plan
$speckit-tasks
$speckit-analyze
$speckit-implement
```

Each step has a different purpose:

1. `$speckit-clarify` resolves missing or ambiguous requirements.
2. `$speckit-plan` defines the React architecture and technical approach.
3. `$speckit-tasks` turns the plan into ordered implementation work.
4. `$speckit-analyze` checks that the specification, plan, and tasks agree.
5. `$speckit-implement` completes the source-code changes.

Optionally, use `$speckit-checklist` before implementation to evaluate areas such as accessibility, testing, and user experience.

The overall process is:

```text
Constitution -> Specification -> Clarification -> Plan -> Tasks -> Analysis -> Implementation
```

Run `specify init` only once for this repository. For each new feature, such as task filters, due dates, or categories, start a new workflow with `$speckit-specify`.

## Test the implementation

After implementation, run:

```powershell
npm test
npm run build
npm start
```

Open [http://localhost:3000](http://localhost:3000) to inspect the application manually.

## Official documentation

- [GitHub Spec Kit](https://github.com/github/spec-kit)
- [Installation guide](https://github.github.com/spec-kit/installation.html)
- [Supported integrations](https://github.com/github/spec-kit/blob/main/docs/reference/integrations.md)
