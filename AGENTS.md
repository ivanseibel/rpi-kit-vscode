# RPI Kit

A toolkit for systematic software development using the **Research → Plan → Implement** (RPI) workflow pattern. RPI Kit installs structured governance, phase-specific constraints, and reusable skills into your repository to guide AI coding agents through disciplined, testable development cycles.

> **⚠️ NAVIGATION:** This file contains content for two different audiences. Use the table below to find what applies to your context.
>
> | Context | Section | Read |
> |---------|---------|------|
> | **You have installed RPI Kit in your repository** | Kit Documentation | [What is RPI Kit?](#a-kit-documentation) → [Quick Start](#quick-start) |
> | **You are working on an RPI project** | Agent Governance | [Agent Roles](#agent-roles-and-responsibilities) → [.rpi/AGENTS.md](.rpi/AGENTS.md) |
> | **You are developing/improving RPI Kit itself** | Developing RPI Kit | [Section B](#b-developing-rpi-kit) |
>

---

## A. Kit Documentation

Universal documentation installed to all target repositories. This section is copied during installation.

<!-- RPI:KIT-DOCUMENTATION:START -->

## What is RPI Kit?

RPI Kit enforces a three-phase development workflow:
- **Research:** Read-only exploration phase with mandatory source citations and FAR validation (Factual, Actionable, Relevant)
- **Plan:** Atomic task decomposition with explicit verification steps following FACTS criteria (Feasible, Atomic, Clear, Testable, Scoped)
- **Implement:** Strict adherence to plan tasks with recursion protocol when blockers arise

Each phase produces artifacts in `.rpi/projects/<project-id>/` with strict handoff gates to prevent premature work.

## Two Flavors

**Codex flavor** (`rpi-kit-codex/`): Repository-backed instruction discovery
- Custom instructions: `AGENTS.md` + `.rpi/AGENTS.md`
- Skills: `.agents/skills/<skill>/SKILL.md`
- Install: `node rpi-kit-codex/install.js --target /path/to/repo`

**Copilot flavor** (`rpi-kit-copilot/`): GitHub Copilot-native integration
- Custom instructions: `.github/copilot-instructions.md` + `.github/instructions/*.instructions.md`
- Prompt entry points: `.github/prompts/rpikit.{research,plan,implement}.prompt.md`
- Skills: `.github/skills/<skill>/SKILL.md`
- Install: `node rpi-kit-copilot/install.js --target /path/to/repo`

## Quick Start

Install RPI Kit into a target repository:

```bash
# Choose your flavor
cd rpi-kit-codex   # or rpi-kit-copilot

# Install with default behavior (skip existing files)
node install.js --target /path/to/target

# Preview changes without writing
node install.js --target /path/to/target --dry-run

# Overwrite all files
node install.js --target /path/to/target --mode overwrite

# Use a config file for per-file overrides
node install.js --target /path/to/target --config rpi-kit.config
```

## Installation Modes

- `skip`: Do not overwrite existing files (default)
- `overwrite`: Replace all existing files
- `prompt`: Ask interactively per file on conflicts

Config file format (line-based key/value):
```
default=skip
file:AGENTS.md=overwrite
file:.github/copilot-instructions.md=prompt
```

## Key Features

### Phase Governance
- Agents assigned roles: Researcher, Planner, Implementer
- Strict artifact boundaries: `research.md`, `plan.md`, implementation files
- Validation criteria enforced at each handoff gate
- Recursion protocol for blockers (stop, document, go back to prior phase)

See [.rpi/AGENTS.md](.rpi/AGENTS.md) for complete agent roles and responsibilities.

### Dependency Discovery
The installer scans instruction files and skills for local markdown links and automatically copies referenced resources. Links resolve from kit root for absolute paths, from file directory for relative paths.

### Template Processing
Templates end with `.rpi-template.md` and include frontmatter:
```markdown
---
target: .github/copilot-instructions.md
---
<!-- RPI:START -->
Content here
<!-- RPI:END -->
```

### Validation Automation
`.github/workflows/rpi-validate.yml` checks required structure exists and enforces governance rules (kit development only; excluded from target repo installation).

## Working with RPI Projects

Once RPI Kit is installed in your repository, you can immediately start RPI projects:

### Starting a New Project
1. Create project directory: `.rpi/projects/<project-id>/`
2. Begin Research phase: Create `research.md`, cite all sources, validate FAR criteria
3. Transition to Plan: Create `plan.md` referencing `research.md` facts, validate FACTS criteria
4. Get sign-off from stakeholders before implementing
5. Implement: Follow `plan.md` tasks strictly, check off completed items, halt if blocked

### Phase Constraints
- **Research**: No solutions proposed, only facts gathered; all sources cited
- **Plan**: Atomic tasks only; each task independently testable; all tasks reference research findings
- **Implement**: No improvisation; verify each task before marking complete; create SIGNOFF file when done

### Recursion Rules
If blocked during any phase:
1. Stop work immediately
2. Document the specific blocker
3. Return to Research (fundamental unknowns) or Plan (execution impossibilities)
4. Do not proceed until prior phase updates

## Skills Available

Both flavors include these skills:
- `rpi-workflow`: Templates, validation helpers, phase transition guidance
- `skill-manager`: Create and edit agent skills following specification
- `e2e-testing`: End-to-end testing patterns (project-specific)
- `github-mcp-issues`: Manage GitHub issues with MCP server tools (Copilot flavor only)

Skills live in `.agents/skills/` (Codex) or `.github/skills/` (Copilot).

## Documentation

Comprehensive docs in `.rpi/docs/`:
- `rpi-workflow.md`: Complete workflow guide
- `rpi-pattern.md`: Pattern overview and rationale
- `rpi-research.md`, `rpi-plan.md`, `rpi-implement.md`: Phase-specific guides
- `handoff-checklist.md`: Validation checklist for phase transitions
- `agent-skills.md`: How to author and use skills
- `custom-instructions.md`: How instructions are loaded and scoped

## Helper Scripts

- `rpi-kit-codex/.rpi/scripts/rpi-worktree-codex.sh`: Codex worktree setup helper
- `rpi-kit-copilot/.rpi/scripts/rpi-worktree-copilot.sh`: Copilot worktree setup helper
- `rpi-kit-copilot/.rpi/scripts/check-vscode-load.sh`: Verify VS Code instruction loading

Scripts are kit-only and excluded from target repo installation.

## Agent Expectations

When working as an agent in a repo with RPI Kit installed:
1. Read `AGENTS.md` and `.rpi/AGENTS.md` to understand your role and constraints
2. Read `.rpi/docs/rpi-workflow.md` before starting any RPI project
3. Always validate artifacts against criteria before phase transitions (FAR for research, FACTS for plans)
4. Follow the recursion protocol strictly if you encounter blockers
5. Never skip phases or improvise solutions outside the plan
6. Load relevant skills from `.agents/skills/` or `.github/skills/` as needed
7. For Copilot flavor: Use prompt entry points `/rpikit.research`, `/rpikit.plan`, `/rpikit.implement` to start phases

## Project Structure After Installation

```
target-repo/
├── AGENTS.md                          # Agent governance rules
├── .rpi/
│   ├── AGENTS.md                      # (identical to root)
│   ├── docs/                          # Workflow documentation
│   └── projects/<project-id>/         # Your RPI projects
│       ├── research.md
│       ├── plan.md
│       └── SIGNOFF
├── .agents/skills/                    # (Codex) or
├── .github/
│   ├── copilot-instructions.md        # (Copilot)
│   ├── instructions/*.instructions.md # (Copilot)
│   ├── prompts/*.prompt.md            # (Copilot)
│   └── skills/                        # (Copilot)
└── .vscode/settings.json              # (Copilot: enables instruction loading)
```

<!-- RPI:KIT-DOCUMENTATION:END -->

---

## B. Developing RPI Kit

> **⚠️ This section applies ONLY to developers working on the rpi-kit repository itself.** If you installed RPI Kit into another repository, you do not need this section. See [Section A](#a-kit-documentation) instead.

### Overview
RPI Kit is developed as two flavors in parallel: `rpi-kit-codex/` and `rpi-kit-copilot/`. Both share governance rules (`.rpi/AGENTS.md` and `.rpi/docs/`) but differ in how they integrate with target repositories.

### Contributing to RPI Kit

When modifying RPI Kit itself:
- Both flavors share `.rpi/AGENTS.md` and `.rpi/docs/` (keep synchronized between `rpi-kit-codex/` and `rpi-kit-copilot/`)
- Kit-specific validation: `.github/workflows/rpi-validate.yml` (validation rules; not installed to targets)
- Test installation: Use `--dry-run` to preview changes before applying
- **Inventory:** Update `INVENTORY.md` when adding/removing artifacts
- **Path specificity:** Each flavor has flavor-specific skills (`.agents/skills/` for Codex, `.github/skills/` for Copilot)

### Folder Structure for Development

```
rpi-kit/
├── AGENTS.md                      # This file: kit docs + development info
├── rpi-kit-codex/
│   ├── AGENTS.md                  # (Codex governance; copied to target root)
│   ├── install.js
│   └── .agents/skills/
├── rpi-kit-copilot/
│   ├── AGENTS.md                  # (Copilot governance; copied to target root)
│   ├── install.js
│   ├── .github/instructions/
│   └── .github/skills/
└── .rpi/
    ├── AGENTS.md                  # Shared governance rules (identical source)
    └── docs/                       # Shared documentation
```

### Testing Kit Changes

1. **Dry run installation:** `cd rpi-kit-codex && node install.js --target /tmp/test-repo --dry-run`
2. **Validate target structure:** Verify target repo has correct file locations
3. **Verify precedence:** Ensure instructions are scoped correctly (Section A goes to root AGENTS.md, governance stays in `.rpi/`)
4. **Test both flavors:** Repeat for `rpi-kit-copilot/`

---

## C. Agent Governance: RPI Workflow

> **ℹ️ These rules apply to all agents working within RPI Kit** (whether in the kit itself or in any repository where RPI Kit is installed). Full governance details are in [.rpi/AGENTS.md](.rpi/AGENTS.md).

This section defines the roles, responsibilities, and constraints for agents operating within the RPI (Research → Plan → Implement) workflow.

## Agent Roles and Responsibilities

| Role        | Allowed Artifacts                                                                     | Mandatory Checks                                                                                                                                                                                                                                                                                                                                                |
| ----------- | ------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Researcher  | `.rpi/**/research.md` (create/edit), read any file for discovery                      | Must cite all sources; Must not propose solutions; Must validate FAR criteria (Factual, Actionable, Relevant); Must scan `.rpi/projects/` for related folders before writing — if a matching folder is found, stop and report it to the operator and wait for explicit authorization; Must not write to an existing project artifact without that authorization |
| Planner     | `.rpi/**/plan.md` (create/edit), read `research.md` and any supporting files          | Must reference `research.md` facts; Must produce atomic, testable tasks; Must validate FACTS criteria (Feasible, Atomic, Clear, Testable, Scoped)                                                                                                                                                                                                               |
| Implementer | Source code, tests, config files (per `plan.md`), update task checkboxes in `plan.md` | Must follow `plan.md` strictly; Must verify each task before marking complete; Must halt if task is impossible and recurse to Planner                                                                                                                                                                                                                           |

## Handoff Rules

1. **Research → Plan:** Planner may not begin until `research.md` exists and passes FAR validation.
2. **Plan → Implement:** Implementer may not begin until `plan.md` exists, passes FACTS validation, and all stakeholders have signed off.
3. **Implement → Review:** Code review may not begin until all tasks in `plan.md` are checked complete and CI validation passes.

## Sign-off Expectations

- **Research Phase:** Planner acknowledges receipt of validated `research.md`.
- **Plan Phase:** Implementer and stakeholders acknowledge receipt of validated `plan.md`.
- **Implementation Phase:** Implementer creates `.rpi/<project>/SIGNOFF` file to signal completion and acceptance of implementation scope.

## Recursion Protocol

If an agent encounters a blocker that cannot be resolved within their phase constraints:

1. **Stop work immediately** - Do not improvise or work around the constraint.
2. **Document the blocker** - Create an issue or update the current phase artifact with the specific blocker.
3. **Recurse to prior phase** - Transition back to Research (for fundamental unknowns) or Plan (for execution impossibilities).
4. **Do not proceed** - Wait for the prior phase to update before resuming.
