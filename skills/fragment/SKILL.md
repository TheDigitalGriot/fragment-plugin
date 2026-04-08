---
name: fragment
description: Scaffold a new Fragment project or add a surface to an existing one using the `fragment` CLI. Use when the user asks to "scaffold a fragment app", "create a fragment project", "add an electron/vscode/tui surface", or invokes `/fragment`.
---

# Fragment Scaffold

Drives the `fragment` CLI (from [fragment-ai-scaffold](https://github.com/TheDigitalGriot/fragment-ai-scaffold)) to scaffold projects and add surfaces. This skill is for **creating** Fragment projects — for **wiring** an existing AI plugin into surfaces, use `/fragment-connect` instead.

## Prerequisites

- `fragment` CLI must be installed and on PATH. Check with `fragment --version`.
- If missing, tell the user to install it (e.g. `npm install -g create-fragment`) rather than trying to work around it.

## CLI Reference

```
fragment init <name> [--electron] [--vscode] [--tui] [--all] [--author <name>]
fragment add  [--electron] [--vscode] [--tui]
fragment connect
```

- `init` creates a **new directory** named `<name>` in the current working directory and scaffolds the requested surfaces. It also runs `npm install` automatically.
- `add` adds a surface to an **existing** Fragment project — must be run from inside the project root.
- `connect` is the CLI equivalent of `/fragment-connect`; prefer the slash command when working inside Claude Code.

## Steps

1. **Parse the user's intent.** Determine:
   - Are they creating a new project (`init`) or adding to an existing one (`add`)?
   - What surfaces do they want? (`electron`, `vscode`, `tui`, or `all`)
   - What name should the project have? (only for `init`)
   - Where should it live? (defaults to current working directory)

2. **Confirm anything ambiguous before running.** `fragment init` creates a directory, installs dependencies, and is mildly destructive if the target name already exists. If the user said "scaffold a fragment app" without specifying surfaces, ask which surfaces they want before running. If they named surfaces clearly (e.g. "electron app called foo"), just run it.

3. **Check the working directory.** `init` should usually run from the parent of where the new project should live. If the user is already inside a Fragment project and asks to add a surface, use `add` instead of `init`.

4. **Note non-standard layouts.** The fragment plugin's `/fragment-connect` flow assumes the AI plugin and the Fragment scaffold live side by side. If the user explicitly says the scaffold won't be colocated with a Claude plugin, **do not** suggest running `/fragment-connect` afterward — the wiring step doesn't apply.

5. **Run the CLI** via Bash. Stream output so the user sees `npm install` progress. Example:
   ```bash
   fragment init my-app --electron
   ```

6. **Report results.** After the command finishes:
   - Confirm the path where the project was created.
   - List the surfaces that were scaffolded.
   - Suggest the next command (`cd <name>` and the dev script) — but do not run it without being asked.
   - Only mention `/fragment-connect` if a colocated AI plugin actually exists.

## Examples

**User:** "scaffold a fragment app called foo with electron and vscode"
→ `fragment init foo --electron --vscode`

**User:** "add a tui surface to this project"
→ Verify cwd is a Fragment project root, then `fragment add --tui`

**User:** "create a fragment project with everything"
→ `fragment init <ask-for-name> --all`

## Things to avoid

- Don't manually scaffold files that the CLI would generate — always defer to `fragment` so the output stays consistent with upstream templates.
- Don't run `npm install` separately; `init` already does it.
- Don't assume `fragment-connect` is the next step. It only applies when an AI plugin is colocated.
