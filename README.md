# OpenSpec Starter Kit

A lightweight starting point for using [OpenSpec](https://openspec.dev) to plan and implement software changes with an AI coding assistant. OpenSpec keeps requirements, design decisions, and implementation tasks alongside your code so that you and your assistant can agree on a change before building it.

## Prerequisites

- Git and an existing project repository (or an empty directory for a new project)
- An AI coding tool supported by OpenSpec, such as Claude Code, Cursor, Codex, or GitHub Copilot
- For the npm installation: Node.js 22.12.0 or later

See the [OpenSpec supported tools](https://openspec.dev/docs/supported-tools) page for current integrations.

## Install the OpenSpec CLI

Choose one installation method.

**macOS or Linux (standalone installer):**

```sh
curl -fsSL https://openspec.dev/cli/install.sh | sh
```

**Windows (PowerShell):**

```powershell
irm https://openspec.dev/cli/install.ps1 | iex
```

**npm (Node.js 22.12.0 or later):**

```sh
npm install -g @fission-ai/openspec@latest
```

Confirm the CLI is available:

```sh
openspec --version
```

If your terminal cannot find `openspec`, follow any PATH instructions printed by the installer, then open a new terminal and try again. For other installation options and troubleshooting, see the [official installation guide](https://openspec.dev/docs/installation).

## OpenSpec setup in this repository

OpenSpec has already been initialized in this repository for **Claude Code**, **Codex**, and **GitHub Copilot** using the `spec-driven` schema. The setup generated six workflows: explore, propose, apply, update, sync, and archive.

Generated project files include:

- `.claude/`: six skills and six slash commands for Claude Code
- `.agents/`: six shared workflow skills used by Codex
- `.github/`: six Copilot prompt files, six skills, and the OpenSpec agent definition
- `.github/workflows/copilot-setup-steps.yml`: GitHub Copilot cloud-agent setup
- `openspec/config.yaml`: project configuration using the `spec-driven` schema, with GitHub Copilot cloud-agent support enabled
- `openspec/specs/` and `openspec/changes/`: current specifications and active/completed changes

Codex uses skills, so separate commands are not installed for it. Commit the generated configuration and workflow files with the repository. If workflows do not appear in your editor, reload it and check the tool-specific instructions below.

### Set up OpenSpec in another project

Install the CLI as described above, then run initialization at the root of the other project:

```sh
cd path/to/your-project
openspec init
```

Select the AI tools used by that project when prompted. OpenSpec creates the `openspec/` directory and the selected tools' workflow files. Running `openspec init` again is safe and can add or refresh tool integrations.

## The OpenSpec change workflow

Each feature or significant change gets its own folder under `openspec/changes/`. The default workflow is:

1. **Explore (optional):** Discuss the problem and possible approaches with your AI assistant before deciding what to build.
2. **Propose:** Ask the assistant to create a proposal, requirements, design (when needed), and task checklist. Review these documents before implementation.
3. **Apply:** Ask the assistant to implement the approved change task by task.
4. **Update the change when the plan needs to change:** Revise its proposal, specs, design, or tasks, then review the updated plan before continuing implementation.
5. **Archive:** Once implementation is complete and the tasks are checked off, archive the change so its requirements are incorporated into the project's main specs.

OpenSpec workflows can be invoked using each tool's command syntax, or by asking the assistant in plain language. For this repository, start a new change with the syntax for your tool:

| Tool | Propose command |
| --- | --- |
| Claude Code | `/opsx:propose "your idea"` |
| Codex CLI or IDE | `$openspec-propose "your idea"` |
| Codex desktop app | Select `openspec-propose` from **Skills** in the sidebar |
| GitHub Copilot | `/opsx-propose "your idea"` |

The command spelling differs by tool; do not assume Claude Code's `/opsx:` syntax works in Copilot or Codex. The examples below use Claude Code syntax where a command is shown. In other tools, invoke the corresponding installed workflow or describe the action in plain language.

### Explore an idea

Use explore when you are still working out the problem, constraints, or options:

```text
Explore how account recovery should work in this app.
```

Or ask your assistant in plain language:

```text
Help me explore how account recovery should work. Review the existing app and compare suitable approaches.
```

Exploration is optional. When the scope is clear, move to propose.

### Propose a change

Start a new change when you know what you want to build:

```text
/opsx:propose "add account recovery"
```

Or ask in plain language:

```text
Propose a change to add account recovery. Include the requirements, edge cases, and implementation tasks.
```

OpenSpec creates a change directory similar to:

```text
openspec/
├── config.yaml
├── specs/                         # Current, accepted requirements
└── changes/
    └── add-account-recovery/
        ├── proposal.md            # Why this change and what it changes
        ├── specs/                 # Requirements introduced or changed
        ├── design.md              # Technical approach, when needed
        └── tasks.md               # Implementation checklist
```

Review the proposal, specs, design, and tasks before asking the assistant to write code. You can edit the Markdown files yourself or ask the assistant to revise them.

### Apply a change

Once the plan is ready, ask the assistant to implement it:

```text
/opsx:apply
```

In Codex, invoke the `openspec-apply-change` skill; in GitHub Copilot, use the installed `opsx-apply` prompt/command or ask in plain language. If there are several active changes, identify the one to apply:

```text
Apply the add-account-recovery change.
```

The assistant uses the change's tasks as its checklist. Review the implementation and run the project's tests, lint, and other relevant checks. If work is interrupted, ask the assistant to apply the same change again; it can continue from the unchecked tasks.

### Update or revise a change

If implementation reveals a gap or the requirements change, do not leave the plan out of date. Ask the assistant to revise the relevant change artifacts before continuing:

```text
Update the add-account-recovery change: include the expired-token behavior in the requirements and add the corresponding implementation and test tasks. Do not implement this revision yet.
```

Review the revised artifacts, then apply the change again. You can also edit the files in the change directory directly. Use the installed `openspec-update-change` workflow in your tool (Claude Code command: `/opsx:update`), or ask in plain language as above.

### Archive a completed change

After implementation is complete and every task is checked off, archive the change:

```text
/opsx:archive
```

Or ask in plain language:

```text
Archive the add-account-recovery change.
```

Archiving moves the completed change under `openspec/changes/archive/` and applies its accepted requirements to `openspec/specs/`. Keep the archive as part of the project's history. For the full archive behavior, see the [OpenSpec quickstart](https://openspec.dev/docs/quickstart).

Use the installed `openspec-archive-change` workflow in Codex or GitHub Copilot; command spelling follows the tool-specific conventions.

## Updating OpenSpec

There are two different kinds of updates:

- **Update the CLI:** Install the latest CLI using the same method you originally chose. For npm, run `npm install -g @fission-ai/openspec@latest`.
- **Refresh this project's generated workflows:** From the project root, run `openspec update`. This refreshes the selected AI tool's OpenSpec skills and commands. Run it in each project that you want to update.

Updating the CLI does not replace the need to refresh generated workflows in your project.

## Useful commands

Run CLI commands from the project root unless the command's help says otherwise:

```sh
openspec list          # List active changes
openspec list --specs  # List current specs
openspec validate      # Validate OpenSpec artifacts
openspec update        # Refresh generated AI workflows for this project
```

For command options and the latest behavior, run `openspec --help` or see the [CLI documentation](https://openspec.dev/docs/cli).

## Project layout

OpenSpec uses `openspec/` for specifications and in-progress changes. Your application source, tests, and other documentation remain in the structure appropriate for your language and framework. A typical initialized project will include:

```text
your-project/
├── .agents/             # Shared agent skills (including Codex)
├── .claude/             # Claude Code skills and commands
├── .github/
│   ├── agents/          # GitHub Copilot agent definition
│   ├── prompts/         # GitHub Copilot workflow prompts
│   ├── skills/          # GitHub Copilot workflow skills
│   └── workflows/       # Copilot cloud-agent setup
├── openspec/
│   ├── config.yaml
│   ├── specs/
│   └── changes/
│       └── archive/
├── src/                 # Your application code, if applicable
├── tests/               # Your tests, if applicable
└── ...
```

## References

- [OpenSpec documentation](https://openspec.dev/docs)
- [Installation](https://openspec.dev/docs/installation)
- [Project setup and initialization](https://openspec.dev/docs/setup)
- [Quickstart](https://openspec.dev/docs/quickstart)
- [Supported AI tools](https://openspec.dev/docs/supported-tools)
- [CLI reference](https://openspec.dev/docs/cli)
