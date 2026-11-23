---
name: revit-setup
description: Connects Claude to Autodesk Revit through pyRevit's built-in Agent Runtime and troubleshoots the connection. Use when the user says "connect to Revit", "Claude can't see Revit", "set up the Revit connector", "pyrevit mcp not working", "which Revit am I connected to", or when any pyRevit tool call fails to reach Revit.
---

# Revit connection setup and troubleshooting

This plugin does not ship its own Revit server. It starts pyRevit's own MCP server (`pyrevit mcp`), which talks to the pyRevit Agent Runtime running inside Revit over a local named pipe. Nothing leaves the user's computer except what Claude reads in the conversation.

## Check the connection first

1. Call `list_revit_instances`. A session with `"responding": true` means the link works.
2. If more than one Revit is running, ask which one to use and pass its year or process id as `revit` on every later call.
3. Call `get_context`. Report back in one line: Revit version, pyRevit version, open model name, active view, and the agent policy (`ask` or `auto`).

## If the tools are missing or nothing responds

Walk the user through these in order and stop at the first fix that works. Keep instructions plain; the user is usually an architect, not a developer.

1. **Revit open with a model** — the agent only answers while Revit is running and has loaded pyRevit.
2. **pyRevit 7 or later installed** — the Agent Runtime was added in pyRevit 7. Installer: https://github.com/pyrevitlabs/pyRevit/releases . In Revit, pyRevit tab → About shows the version.
3. **Agent enabled** — pyRevit tab → Settings → Agent (or the `[agent]` section of `%APPDATA%\pyRevit\pyRevit_config.ini`): `enabled = true`. Recommend `policy = "ask"` so every model change shows an approval prompt in Revit.
4. **`pyrevit` command found** — the plugin launches `pyrevit mcp`. If Windows can't find `pyrevit`, the pyRevit CLI is not on PATH. Fix by reinstalling pyRevit with the CLI option, or by editing the plugin's server command to the full path, typically `%APPDATA%\pyRevit-Master\bin\pyrevit.exe`.
5. **Restart** — after changing settings, restart Revit, then restart the Claude desktop app.
6. **Duplicate server** — if the user also added `pyrevit mcp` by hand in Claude's settings, two copies may run. Suggest removing the manual entry and keeping the plugin's.

## Policy explained to users

- `ask`: Revit shows each change, isolates the affected elements, and waits for Accept or Discard. Kept changes become one Undo step named "Agent: <title>". Recommend this for everyone.
- `auto`: changes are committed without a prompt. Only for experienced users on backed-up models.
