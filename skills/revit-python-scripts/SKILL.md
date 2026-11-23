---
name: revit-python-scripts
description: Writes and runs custom Python scripts inside Revit through pyRevit for tasks no ready-made tool covers — complex queries, batch modeling changes, view and sheet automation, model checks — and turns a working script into a reusable pyRevit ribbon button. Use for "write a script that…", "automate this in Revit", "run Python in Revit", "make this a pyRevit button", "batch create…".
---

# Custom Python scripts in Revit

Scripts run inside the user's live Revit session with full access to the model and the computer. Treat them with care.

## Before writing code

1. `get_context` — note `scripting.default_engine` and its syntax limits. The default IronPython engine is Python 2.7: no f-strings, no type hints, no `nonlocal`, no walrus. Pass `engine: "cpython"` only when a CPython-only library is needed, and know that `pyrevit.forms` may be limited there.
2. `get_skill` → `revit-scripting` (always), `pyrevit-library`, then the task skill: `modeling`, `views`, `drawings`, `scheduling`, `family-editing`, `revit-files`, or `extension-authoring`.
3. Check `list_automation` — a reviewed operation may already do it.
4. Use `lookup_revit_api` / `lookup_pyrevit_api` rather than guessing member names.

## Running

- Read-only work → `run_query`. Any change is rolled back automatically.
- Changes → `run_modify`, always `dry_run: true` first, then the real run with a clear `title`. The user approves in Revit under policy `ask`; the result is one Undo step.
- Keep each run small and re-runnable. For multi-step builds, put shared helpers in a workspace folder and pass `workspace` so later runs can `import` them.
- Return structured data via `result`; keep prints short.
- Default `timeout_s` is 300; raise it only for known long jobs, and warn the user.
- If a run errors, read the traceback, fix the specific cause, and retry once or twice. Don't loop.

## Hard limits

- Do not run scripts that delete files, call the network, read credentials, change Windows settings, or install software.
- Do not delete model elements or purge unless the user asked for exactly that and confirmed the list.
- Guarded runs cannot save or export documents; tell the user to save from Revit.
- Never switch the agent policy to `auto` on the user's behalf.

## Making it a button

When a script works and the user wants to keep it, follow the `extension-authoring` skill: create a bundle in the agent-made extensions folder, convert the script into a button `script.py` with a `bundle.yaml`, reload pyRevit, and confirm the button appears.
