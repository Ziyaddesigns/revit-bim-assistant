# Revit BIM Assistant

A Claude plugin for architects, interior designers and BIM teams. It lets Claude work inside Autodesk Revit through **pyRevit's built-in Agent Runtime**:

- **Read the model**: levels, rooms, element counts, quantities, parameters and linked models
- **Shop drawings**: create sheets, place views and schedules, renumber sheets, and export a drawing log or submittal register to Excel
- **Edit parameters**: single or bulk changes, previewed first and approved in Revit, with one Undo step per change
- **Python scripts**: custom automation, and turning working scripts into pyRevit ribbon buttons

This plugin contains instructions (skills) and a launcher entry only. It does not include or modify pyRevit's code. The Revit connection is pyRevit's own `pyrevit mcp` server, made by the pyRevit project (https://github.com/pyrevitlabs/pyRevit).

## Requirements

- Windows with Autodesk Revit (any version supported by your pyRevit build)
- pyRevit 7 or later, with the command-line tool (`pyrevit`) on PATH
- The Claude desktop app

## Setup

1. Install pyRevit 7+ from https://github.com/pyrevitlabs/pyRevit/releases
2. In Revit, go to pyRevit → Settings → Agent and enable the agent. Keep the policy on **ask** so every model change needs your approval in Revit.
3. Install this plugin in Claude. In Claude Code, run:
   ```
   /plugin install revit-bim-assistant --marketplace ziyaddesigns/revit-bim-assistant
   ```
   In the Claude desktop app, add `ziyaddesigns/revit-bim-assistant` as a plugin marketplace and install the plugin from it.
4. Restart Revit and the Claude desktop app, open a model, and ask Claude: *"Check the Revit connection."*

If Claude can't find `pyrevit`, change the server command in `.mcp.json` to the full path, usually:
`C:\Users\<you>\AppData\Roaming\pyRevit-Master\bin\pyrevit.exe`

If you already added `pyrevit mcp` by hand in Claude's settings, remove that entry so it doesn't run twice.

## Safety

- Read-only questions run through `run_query`, which rolls back any change automatically.
- Every model change is first previewed as a dry run. Under the **ask** policy, Revit then shows the changed elements and waits for you to Accept or Discard.
- Kept changes appear as a single Undo step named "Agent: …".
- Agent runs cannot save or export your files. You always save from Revit yourself.
- Custom scripts run with your Windows user's permissions. Review what Claude proposes before you approve it.

## Privacy Policy

**Data collection.** The plugin itself collects nothing and has no server. It runs `pyrevit mcp` locally on your computer, which talks to Revit over a local named pipe.

**Usage and storage.** Model information that Claude reads (element data, parameter values, view captures) is sent to Claude as part of your conversation and is handled under Anthropic's privacy policy (https://www.anthropic.com/legal/privacy). pyRevit keeps local run records and view captures under `%APPDATA%\pyRevit\agent` on your computer.

**Third-party sharing.** The plugin author receives no data. Nothing is sent anywhere except the Claude conversation you are having.

**Data retention.** Local pyRevit run records stay on your computer until you delete them. Conversation retention follows your Claude account settings.

**Contact.** Ziyad Faraj, via https://www.linkedin.com/in/ziyaddesigns

## Credits

The Revit connection and the Agent Runtime are part of pyRevit by pyRevitLabs. This plugin is an independent project and is not affiliated with or endorsed by pyRevitLabs or Autodesk. Revit is a trademark of Autodesk, Inc.

## License

MIT. See LICENSE.
