# Inventory Import PRD — error & state copy

Companion to [prd-inventory-import.md](prd-inventory-import.md): the shipping copy for every state that PRD names.
State IDs are local and stable; E4–E14, E38 and E40 retain their inherited numbers, and E41/E42 are new in this amendment. [Capture §12](../capture-mode/prd-capture-mode.md#12-error--state-copy) owns labels/placeholders; [the Legend](prd-inventory-import.md#legend) owns statuses; [R3.8](prd-inventory-import.md#3-preview-and-commit) owns action transitions.

## Error & state copy

Variants share a state ID; tests can distinguish the cause without matching text. All offered actions below are observable under R4.1.

| ID | State | Headline | Body | Offered actions | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| E4 | No header row | Which row has your column names? | This file doesn't seem to start with a row of column names. Pick the row that does, or name the columns yourself. | Pick the header row; Name columns; Cancel | 🤝 Aligned |
| E5 | Can't read the file | Can't read this file | Text variant: Choose the text encoding and separator used by your spreadsheet, or save the file again as CSV. Syntax variant: The quoted fields cannot be read with these settings. Check the separator or save the file again from your spreadsheet. Nothing has been imported. | Choose an encoding; Choose a separator; Pick the file again; Cancel | 🤝 Aligned |
| E6 | Nothing to import | This file has column names but no rows | There's nothing to bring in. Check you picked the right file. | Pick a different file; Cancel | 🤝 Aligned |
| E7 | Rows with the wrong number of columns | ⟨n⟩ rows don't line up with the columns | These rows have more or fewer values than the header row, so they're listed below and left out. You can bring in the rest, or fix the file and try again. | Continue without them; Pick the file again; Cancel | 🤝 Aligned |
| E8 | Mapping incomplete | Tell the app which column is the swatch code | Every swatch needs a code — it's how the app matches a row when you re-import, and how you find a swatch later. | Choose the code column; Cancel | 🤝 Aligned |
| E9 | Blank codes excluded | ⟨n⟩ rows have no swatch code | Rows without a code are listed below and left out, because there'd be no way to find them again. Bring in the rest, or fill the codes in and try again. | Continue without them; Pick the file again; Cancel | 🤝 Aligned |
| E10 | Duplicate codes in the file | ⟨n⟩ rows share a swatch code | Every row listed below is left out — the app won't guess which one you meant. Spacing and capitals don't count as a difference, so "CG 3" and "cg  3" — the same letters with an extra space — are the same code. Bring in the rest, or fix the file and try again. | Continue without them; Pick the file again; Cancel | 🤝 Aligned |
| E11 | Ambiguous code | ⟨code⟩ matches more than one swatch in this collection | This row is left out. None of the matching swatches will change; you can review and import the other rows. | Continue without it; Cancel | 🤝 Aligned |
| E12 | Unnamed column | ⟨n⟩ columns have no name | Their generated names and column positions are listed below. You can rename them later when browsing the collection. | Continue; Cancel | 🤝 Aligned |
| E13 | File changed on disk | This file changed while you were looking at it | The app has read it again. Check the columns and review the updated rows before importing. | Review again; Cancel | 🤝 Aligned |
| E14 | Changed metadata on a captured row | ⟨n⟩ swatches you've already scanned have different details in this file | The app will take the new details, because a corrected spreadsheet is the usual reason to import again — change that here, for all of them or one at a time. Where the file leaves a field empty, taking the new details empties that field too; those are listed below. Your measurements aren't touched either way. | Take the new details; Keep what I have; Cancel | 🤝 Aligned |
| E38 | File gone before the import | That file isn't where it was | It's been moved, renamed, or deleted since you picked it. Nothing has been imported. Pick it again and the app will read it fresh. | Pick the file again; Cancel | 🤝 Aligned |
| E40 | Import blocked by a session | Finish your session in ⟨collection⟩ first | A session is open here, so new swatches can't join its queue mid-run. End or finish the session, then import — nothing about the file has changed. | Go to the session; End that session; Cancel | 🤝 Aligned |
| E41 | Duplicate column names | These column names match | The listed column names match each other. Give them distinct names in your spreadsheet, then pick the file again; nothing has been imported. | Pick the file again; Cancel | 🤝 Aligned |
| E42 | No eligible rows | No rows can be imported | Every row is listed below with the reason it was left out. Fix the file and pick it again; nothing has been imported. | Pick the file again; Cancel | 🤝 Aligned |
