---
name: tex-toolchain-compile
description: Use when LaTeX work has compile failures, missing binaries, PATH issues, stale build artifacts, or log-driven compile-fix loops before higher-level edits.
---

# Tex Toolchain Compile

## Overview
Use this skill to stabilize the compile layer before doing semantic edits. Prefer deterministic diagnosis first, then minimal patches tied to concrete log errors.

## Executable Entry Points
- `scripts/check_toolchain.py`: checks required binaries (`latexmk`, TeX engine, Poppler tools) and emits machine-readable JSON.
- `scripts/extract_compile_issues.py`: parses LaTeX compile output or `.log` files into normalized errors/warnings.

Example commands:
```bash
python scripts/check_toolchain.py --pretty --fail-on-missing
python scripts/extract_compile_issues.py --log-file build/main.log --pretty
```

## Project Adaptation
1. Identify your project's compile entrypoint (command, script, or task runner).
2. Identify where logs and artifacts are written.
3. Identify required binaries (typically `latexmk`, TeX engine, `pdfinfo`, `pdftoppm`).

## Workflow
1. Run `scripts/check_toolchain.py` to verify required binaries on `PATH`.
2. Run compile once to get baseline errors.
3. If stale artifacts are suspected, run a clean build and compile again.
4. Run `scripts/extract_compile_issues.py` and parse errors in priority order:
   - fatal lines (`! ...`)
   - `LaTeX Error:`
   - `Package ... Error:`
5. Generate minimal unified diff patches only for directly failing lines.
6. Recompile immediately after each applied patch batch.
7. Stop when compile succeeds or when errors are non-local (missing assets, external packages, broken toolchain install).

## Inputs and Outputs
- Input: main TeX entry file, toolchain status, compile command output, `.log` content.
- Output: compile status, concise error summary, and minimal patch set (if applicable).

## Common Mistakes
- Debugging paper content before fixing toolchain/binary issues.
- Applying broad speculative rewrites instead of log-targeted fixes.
- Not recompiling after each patch batch.
- Ignoring fallback logs when stdout/stderr is truncated.
