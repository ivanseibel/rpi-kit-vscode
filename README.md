# RPI Kit

This folder packages the RPI workflow artifacts so they can be copied into another repository.

## Kit Root

- Kit root: rpi-kit (copyable folder)

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
