# Install Paper Skills (for Codex)

Run the following command in the **current project root** to install this skill set locally:

```bash
set -euo pipefail
TMP_DIR="$(mktemp -d)"
git clone --depth 1 https://github.com/diggerdu/paper-skills.git "$TMP_DIR"
mkdir -p .agents/skills
cp -a "$TMP_DIR/.agents/skills/." .agents/skills/
rm -rf "$TMP_DIR"
```

This installs the 4 portable LaTeX skills into `./.agents/skills` (project-local, not global).

After installation, restart Codex in this project so it can discover the new skills.
