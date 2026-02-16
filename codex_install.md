# Paper Skills for Codex

Guide for installing portable LaTeX skills in any Codex project via native skill discovery.

## Quick Install

Tell Codex:

```
Fetch and follow instructions from https://raw.githubusercontent.com/diggerdu/paper-skills/main/codex_install.md
```

## Manual Installation

### Prerequisites

- OpenAI Codex CLI
- Git
- Bash-compatible shell

### Steps

1. Run in your target project root:
   ```bash
   set -euo pipefail
   TMP_DIR="$(mktemp -d)"
   git clone --depth 1 https://github.com/diggerdu/paper-skills.git "$TMP_DIR"
   mkdir -p .agents/skills
   cp -a "$TMP_DIR/.agents/skills/." .agents/skills/
   rm -rf "$TMP_DIR"
   ```
2. Restart Codex.

## What Gets Installed

These project-local skills are copied into `./.agents/skills`:

- `tex-toolchain-compile`
- `tex-latex-structure-parser`
- `tex-citation-validate-fix`
- `tex-figure-table-section-fix`

## How It Works

Codex discovers skills from `.agents/skills` at startup. After installation, these skills become available only in the current project.

## Updating

Re-run the installation command to pull the latest skill content.

## Uninstalling

Remove the installed folders from your project:

```bash
rm -rf .agents/skills/tex-toolchain-compile \
       .agents/skills/tex-latex-structure-parser \
       .agents/skills/tex-citation-validate-fix \
       .agents/skills/tex-figure-table-section-fix
```

## Troubleshooting

### Skills not showing up

1. Verify folders exist: `find .agents/skills -maxdepth 1 -type d`
2. Restart Codex (skills are loaded at startup)
3. Confirm `SKILL.md` files exist under each skill folder
