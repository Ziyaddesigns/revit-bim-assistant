---
name: revit-model-info
description: Reads and summarizes information from the open Revit model without changing it — levels, views, sheets, rooms, element counts, quantities, family and type lists, parameter values, the current selection, and linked models. Use for "what's in this model", "count the doors by type", "list all rooms with areas", "what are the parameters of the selected element", "show me the kitchen", "quantity takeoff", "audit this model".
---

# Reading the Revit model

All reading uses pyRevit tools that cannot change the model. Never use `run_modify` in this skill.

## Workflow

1. **Orient.** Call `get_context` once per conversation (and again if the user switches models). It returns the Revit version, open documents, active view, selection, levels, and the script engine and Python syntax to use.
2. **Learn the rules before the first script.** Call `get_skill` with `revit-scripting`, then `pyrevit-library`. Use `scheduling` for quantities and `views` for anything about views.
3. **Prefer ready-made operations.** Call `list_automation` and use `run_automation` when one fits; it needs no code.
4. **Otherwise write a query.** Use `run_query` with a short, descriptive `title`. Assign the output to `result` as plain lists and dicts. Respect the engine's syntax: the default IronPython engine is Python 2.7 (no f-strings; use `%` or `.format`).
5. **Look up APIs instead of guessing.** Use `lookup_revit_api` or `lookup_pyrevit_api` when unsure of a class or method name.
6. **Show, don't just tell.** `show_elements` selects and zooms to element ids in Revit; `capture_view` returns a picture of a view; `inspect_elements` gives full detail on specific ids.
7. **Linked models.** Use `navigate_revit_link` to read elements inside links.

## Query habits

- Use `FilteredElementCollector(doc).OfCategory(...).WhereElementIsNotElementType()` for instances; `WhereElementIsElementType()` for types.
- Convert internal units before reporting. Revit stores length in feet and area in square feet; report in the project's display units (check with `doc.GetUnits()`), usually millimetres and square metres in the Gulf region.
- Cap large outputs: return counts plus the first 200 rows, and offer the rest as a spreadsheet.
- Read parameters by built-in parameter where possible; fall back to `LookupParameter("Name")` for shared/project parameters, and report when a parameter is missing rather than failing.

## Reporting

- Answer in a short summary, then a table if there are more than a few rows.
- For anything longer than ~30 rows, or when the user wants to keep it, build a spreadsheet instead of pasting rows into chat.
- Mention element ids so the user can find items in Revit (Manage → Select by ID), and offer `show_elements`.
