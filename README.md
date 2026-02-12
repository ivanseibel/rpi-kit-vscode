# RPI Kit

This folder packages the RPI workflow artifacts so they can be copied into another repository.

## Kit Root

- Kit root: rpi-kit (copyable folder)

## How the RPI Workflow Is Implemented

This kit implements the RPI workflow primarily through **GitHub Copilot customization** (custom instructions, prompt entry points, and skills), backed by repo-local governance docs and CI validation.

What gets installed and how it works:

- **Global Copilot custom instructions:** `.github/copilot-instructions.md` provides the RPI constitution and the high-level “Research → Plan → Implement” rules.
- **Scoped instruction files (phase constraints):** `.github/instructions/*.instructions.md` uses `applyTo:` to enforce stricter rules for specific artifacts (for example, `.rpi/projects/**/research.md` and `.rpi/projects/**/plan.md`).
- **Prompt entry points:** `.github/prompts/rpikit.{research,plan,implement}.prompt.md` gives you `/rpikit.research`, `/rpikit.plan`, and `/rpikit.implement` in Copilot Chat as a consistent way to start each phase.
- **Copilot skills:** `.github/skills/**/SKILL.md` packages reusable “how to do X” playbooks that Copilot can load alongside instructions (for example, `rpi-workflow` for templates/validation guidance, plus any project-specific skills you want to ship with the kit).
- **VS Code wiring:** `.vscode/settings.json` enables instruction file loading (`github.copilot.chat.codeGeneration.useInstructionFiles`).
- **Governance + artifacts:** `AGENTS.md` (and `.rpi/AGENTS.md`) defines roles, handoffs, and recursion rules; `.rpi/` holds docs, example projects, and helper scripts.
- **Validation automation:** `.github/workflows/rpi-validate.yml` checks that the required files and structure exist (instructions, prompts, skills, docs, governance) so the workflow remains consistent over time.

## Install

```bash
node install.js --target /path/to/target
```

**Requirements by Platform:**

- **Linux**: Node.js 16+ (or newer) installed. Run directly:
  ```bash
  node install.js --target /path/to/target
  ```

- **macOS**: Node.js 16+ (or newer) installed:
  ```bash
  node install.js --target /path/to/target
  ```

- **Windows**: Node.js 16+ (or newer) installed:
  ```powershell
  node install.js --target C:\path\to\target
  ```

**Shell wrapper:**

If you prefer, you can also run the POSIX wrapper:

```bash
sh install.sh --target /path/to/target
```

### Options

- --target <path>   Target repository root (default: current directory)
- --mode <mode>     overwrite behavior: skip | overwrite | prompt (default: skip)
- --config <path>   optional config file with per-file overrides
- --dry-run         show planned file operations without writing

## Overwrite Modes

- skip: do not overwrite existing files
- overwrite: replace existing files
- prompt: ask per file on conflicts

## Config File

The config file is line-based key/value pairs:

```
# rpi-kit.config
# default mode for all files
default=skip

# per-file overrides
file:.github/copilot-instructions.md=overwrite
file:.vscode/settings.json=prompt
```

## Dependency Discovery

The installer copies the core kit roots and also scans instruction and skill markdown files for local link targets. Any referenced local paths found are added to the copy list if they exist in the kit. Rules:

- Absolute links like /path/to/file resolve from the target repo root.
- Relative links resolve from the referencing file directory.
- Links with http, https, mailto, or # anchors are ignored.

## Template Processing

Templates live in rpi-kit/templates and end with .rpi-template.md. Each template must include frontmatter with a target path and content markers.

Example:

```markdown
---
target: .github/copilot-instructions.md
---
<!-- RPI:START -->
RPI-only content
<!-- RPI:END -->
<!-- PROJECT:START -->
Project-specific content
<!-- PROJECT:END -->
```

During install, only the RPI section is written to the target file. Project-specific sections are omitted unless you customize the template handling.

## Notes

- The installer only copies the RPI kit artifacts and referenced dependencies.
- The kit includes validation workflow, instructions, prompts, skills, and RPI docs.
