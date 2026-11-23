# Sheet numbering and drawing log conventions

Use the user's own convention if they give one or if it can be read from existing sheets. Otherwise propose this default and confirm it before creating sheets.

## Default sheet number pattern

`<Discipline>-SD-<Series><Seq>`

| Series | Content |
|--------|---------|
| 000 | Cover, drawing list, general notes, legends |
| 100 | Floor plans / setting-out plans |
| 200 | Reflected ceiling plans |
| 300 | Elevations |
| 400 | Sections |
| 500 | Details |
| 600 | Joinery / millwork items |
| 700 | Schedules (materials, finishes, ironmongery) |

Discipline prefixes: `A` architectural, `ID` interior, `J` joinery, `S` structural, `M`/`E`/`P` MEP.

Example: `ID-SD-301` = interior shop drawing, first elevation sheet.

When the project has a project code (e.g. `KH24-434`), it usually goes in the title block's project number field, not in the sheet number. Ask before adding it to sheet numbers.

## Drawing log columns

| Column | Source |
|--------|--------|
| Project code | Project Information → Project Number |
| Sheet number | Sheet Number |
| Sheet title | Sheet Name |
| Discipline | Prefix of sheet number, or a Discipline parameter |
| Revision | Current Revision |
| Revision date | Current Revision Date |
| Revision description | Current Revision Description |
| Submitted date | User-supplied |
| Status | User-supplied (e.g. A – Approved, B – Approved as noted, C – Revise and resubmit, D – Rejected) |
| Consultant comments | User-supplied |
| Next action / due | User-supplied |

Keep one row per sheet per revision when the user wants history; otherwise one row per sheet showing the latest revision.
