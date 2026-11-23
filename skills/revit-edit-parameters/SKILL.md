---
name: revit-edit-parameters
description: Changes parameter values on Revit elements, singly or in bulk, with a preview and user approval in Revit — marks, comments, type names, finishes, room names and numbers, sheet parameters, shared parameters, and values copied from a spreadsheet. Use for "renumber the doors", "fill in the Comments for these", "set the finish on all selected", "update parameters from this Excel", "rename rooms", "copy Mark to another parameter".
---

# Editing parameters

## Workflow

1. **Identify the targets.** Use the current selection (`get_context` → `selection`) or a `run_query` that collects elements by category, type, level or filter. Tell the user how many elements matched before changing anything. If the count is surprising, stop and ask.
2. **Read current values.** `run_query` the parameter on every target. Report: parameter found on N elements, missing on M, read-only on K. Instance vs type: changing a type parameter changes every instance of that type — say so explicitly.
3. **Preview.** Call `run_modify` with `dry_run: true`. Show a short before → after table (first 10–20 rows plus totals).
4. **Apply.** Call `run_modify` with a specific `title` ("Set Comments on 48 doors"). Under policy `ask` the user approves in Revit; the change becomes one Undo step. If the status is `rejected`, ask what to change.
5. **Verify.** Re-read the values with `run_query` and report any element that didn't take the new value, with its id.

## Rules for the script

- Open exactly one `DB.Transaction` with a meaningful name; set values inside it.
- Lengths and areas: Revit stores feet / square feet internally. Convert from the user's units, or use `Parameter.SetValueString("3000 mm")` for unit-aware input.
- Check `param.IsReadOnly` and the `StorageType` (String, Integer, Double, ElementId) before setting; skip and report instead of failing the whole run.
- Yes/No parameters are Integer 1/0.
- Never delete elements, purge, or change worksharing/ownership in this skill.
- Workshared models: elements owned by someone else can't be edited; report them by id.

## From a spreadsheet

When the user provides an Excel or CSV: match rows by a stable key (element id, Mark, or Room Number), never by row order. Show unmatched rows on both sides before applying.
