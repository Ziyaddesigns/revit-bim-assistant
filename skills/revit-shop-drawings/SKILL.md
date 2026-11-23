---
name: revit-shop-drawings
description: Produces and manages shop drawing deliverables in Revit — creating sheets with title blocks, placing plans, elevations, sections and schedules on sheets, renumbering sheets, building material and millwork schedules, and exporting a shop drawing register or drawing log to Excel. Use for "set up shop drawing sheets", "make a joinery schedule", "place these elevations on sheets", "renumber sheets", "export the drawing list", "update the shop drawing log", "prepare sheets for submittal".
---

# Shop drawings, schedules and drawing logs

Shop drawings for fit-out and millwork follow a predictable package: cover and drawing list, plans, reflected ceiling plans, elevations, sections, details, and material/finish schedules. This skill sets them up in Revit and keeps the drawing log in step.

## Before starting

1. Call `get_context`, then `get_skill` for `revit-scripting`, `drawings`, and `scheduling`.
2. Ask only what the model can't tell you:
   - Project code and sheet numbering pattern (see `references/numbering-and-log.md`).
   - Which title block family to use, if the model has more than one.
   - Revision number and date for this submittal.
3. Read what exists first with `run_query`: sheets (number, name, current revision), title blocks loaded, views not yet placed on sheets, existing schedules.

## Making changes

Every change goes through `run_modify`:

1. Run with `dry_run: true` first and tell the user in one or two lines what will be created or changed (e.g. "12 new sheets A-SD-101 to A-SD-112, 24 views placed").
2. Then run for real with a clear `title` such as "Create SD sheets 101–112". Under policy `ask`, the user approves in Revit. A status of `rejected` means they discarded it — ask what to change, don't retry the same thing.
3. Create in stages (sheets → place views → schedules → tags) so each approval is small and easy to check.
4. After placing views, `capture_view` one or two sheets so the user can check layout.

## Schedules

- Millwork/joinery schedule fields: Mark, Type, Description, Material/Finish, Width, Height, Depth, Count, Room, Level, Comments.
- Material/finish schedule fields: Material code, Description, Supplier, Finish, Area or Count, Location.
- Sort and group by Level then Room; turn on totals for counts and areas.
- Read schedule contents back with `run_query` to confirm before placing on sheets.

## Drawing log / submittal register

When the user asks for a drawing list, register, or to update their log:

1. Query all sheets: number, name, current revision, revision date, sheet issue date, discipline, and any custom status parameter.
2. Produce a spreadsheet with the columns in `references/numbering-and-log.md`. If the user already has a master log, ask for the file and update matching rows by sheet number rather than replacing it; add new sheets at the end of their discipline group and flag sheets in the log that no longer exist in the model.
3. Don't invent statuses or approval codes. Leave them blank for the user, or copy them from their existing log.

## Exporting

Guarded agent runs cannot save or export documents. For PDF/DWG export, prepare a print set or sheet selection and tell the user to export from Revit (File → Export / Print), or offer to make a pyRevit button for it using the `extension-authoring` skill.
