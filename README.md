# Fragment Plugin

A Claude Code plugin that wires AI plugins into [Fragment](https://github.com/TheDigitalGriot/fragment-ai-scaffold)-scaffolded surfaces (Electron, VS Code, TUI).

## Installation

Add to your Claude Code plugins directory or include as a submodule:

```bash
git submodule add git@github.com:TheDigitalGriot/fragment-plugin.git plugins/fragment-plugin
```

## Skills

### `/fragment-connect`

Wires an AI plugin into Fragment surfaces. Detects which surfaces exist in `apps/`, reads the plugin's `.claude-plugin/plugin.json` manifest, and generates glue code:

- **Electron** — MCP server IPC handlers, tool timeline wiring, renderer commands
- **VS Code** — Contributed commands, tree view providers, timeline panel integration
- **TUI** — Plugin tab implementations, event bus wiring, timeline pane

### `/fragment-status`

Reports the current wiring state of a Fragment project — which surfaces exist, which MCP servers are connected, hook status, and model availability (Claude, Codex, Gemini).

## Agents

### Connector Agent

Handles complex wiring logic when a plugin has multiple MCP servers, non-standard tool configurations, or requires custom glue code. Invoked automatically by `/fragment-connect` when needed.

## Scripts

### `detect-surfaces.py`

Detects which Fragment surfaces exist in the current project and outputs JSON with surface paths, status, and any discovered plugin manifests.

```bash
python scripts/detect-surfaces.py /path/to/project
```

## Structure

```
.claude-plugin/
  plugin.json        # Plugin manifest
agents/
  connector-agent.md # Complex wiring agent
scripts/
  detect-surfaces.py # Surface detection
skills/
  fragment-connect/  # Wire plugins into surfaces
  fragment-status/   # Report wiring state
```

## License

MIT
