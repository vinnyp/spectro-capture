# Inventory Import PRD — error & state copy

Companion to [prd-inventory-import.md](prd-inventory-import.md): the shipping copy for every state that PRD names.
State IDs are local and stable; E4–E14, E38 and E40 retain their inherited numbers, and E41–E45 are new in this amendment. [Capture §12](../capture-mode/prd-capture-mode.md#12-error--state-copy) owns labels/placeholders; [the Legend](prd-inventory-import.md#legend) owns statuses; [R3.8](prd-inventory-import.md#3-preview-and-commit) owns action transitions.

## Error & state copy

Variants share a state ID; tests can distinguish the cause without matching text. Variant labels below are documentation, not displayed text; render only the applicable body and actions. All offered actions below are observable under R4.1. Every state offers Cancel before commit; commit itself finishes or rolls back.

| ID | State | Headline | Body | Offered actions | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| E4 | No header row | Which row has your column names? | This file doesn't seem to start with a row of column names. Pick the row that does, or name the columns yourself. | Pick the header row; Name columns; Cancel | 🤝 Aligned |
| E5 | Can’t read the file | Can’t read this file | Text: Choose the text encoding and separator used by your spreadsheet, or save the file again as CSV. Syntax: The quoted fields cannot be read with these settings. Check the separator or save the file again from your spreadsheet. All listed encodings failed: None of the available encodings could read this file. Save it again as UTF-8 CSV, then pick the file again. All variants append: Nothing has been imported. | Text/syntax: Choose an encoding; Choose a separator; Pick the file again; Cancel. All encodings failed: Pick the file again; Cancel | 🤝 Aligned |
| E6 | Nothing to import | This file has no rows to import | There’s nothing to bring in. Check you picked the right file. | Pick a different file; Cancel | 🤝 Aligned |
| E7 | Rows with the wrong number of columns | ⟨n⟩ rows don't line up with the columns | These rows have more or fewer values than the header row, so they're listed below and left out. You can bring in the rest, or fix the file and try again. | Continue without them; Pick the file again; Cancel | 🤝 Aligned |
| E8 | Mapping incomplete | Tell the app which column is the swatch code | Every swatch needs a code — it's how the app matches a row when you re-import, and how you find a swatch later. | Choose the code column; Cancel | 🤝 Aligned |
| E9 | Blank codes excluded | ⟨n⟩ rows have no swatch code | Rows without a code are listed below and left out, because there'd be no way to find them again. Bring in the rest, or fill the codes in and try again. | Continue without them; Pick the file again; Cancel | 🤝 Aligned |
| E10 | Duplicate codes in the file | ⟨n⟩ rows share a swatch code | Every row listed below is left out — the app won't guess which one you meant. Spacing and capitals don't count as a difference, so "CG 3" and "cg  3" — the same letters with an extra space — are the same code. Bring in the rest, or fix the file and try again. | Continue without them; Pick the file again; Cancel | 🤝 Aligned |
| E11 | Ambiguous code | ⟨code⟩ matches more than one swatch in this collection | This row is left out. None of the matching swatches will change; you can review and import the other rows. | Continue without it; Cancel | 🤝 Aligned |
| E12 | Unnamed column | ⟨n⟩ columns have no name | Their positions and generated names are listed below. You can rename them later when browsing the collection. | Continue with the listed names; Pick the file again; Cancel | 🤝 Aligned |
| E13 | File changed on disk | This file changed while you were looking at it | The app has read it again and reset your choices about changed details. Check the columns and review the updated rows before importing. | Review again; Cancel | 🤝 Aligned |
| E14 | Changed metadata on a captured row | ⟨n⟩ swatches you’ve already scanned have different details in this file | The app will take the new details; change that here for all of them or one at a time. Taking an empty field clears its old value; those changes are listed below. Keeping your details preserves existing fields, including empty ones; new fields still get the file’s values. Your measurements aren’t touched either way. | Take the new details; Keep what I have; Cancel | 🤝 Aligned |
| E38 | File gone before the import | That file isn't where it was | It's been moved, renamed, or deleted since you picked it. Nothing has been imported. Pick it again and the app will read it fresh. | Pick the file again; Cancel | 🤝 Aligned |
| E40 | Import blocked by a session | Finish your session in ⟨collection⟩ first | A session is open here, so new swatches can't join its queue mid-run. End or finish the session, then import — nothing about the file has changed. | Go to the session; End that session; Cancel | 🤝 Aligned |
| E41 | Duplicate column names | These column names match | The positions, original names and distinct names the app will use are listed below. You can continue with these names or change the file and pick it again. | Continue with the listed names; Pick the file again; Cancel | 🤝 Aligned |
| E42 | No eligible rows | No rows can be imported | Every row is listed below with the reason it was left out. Fix the file and pick it again; nothing has been imported. | Pick the file again; Cancel | 🤝 Aligned |
| E43 | Import preview | Import into ⟨collection⟩ | Target: ⟨collection⟩ — ⟨existing⟩ existing items. Encoding: ⟨encoding⟩. Separator: ⟨delimiter⟩. New: ⟨new⟩. Updated: ⟨updated⟩. Unchanged: ⟨unchanged⟩. Added columns: ⟨columns⟩. Left out: ⟨excluded⟩ rows (listed with reasons). ⟨absent⟩ rows in the collection are not in this file — left untouched. Above-ceiling variant appends: This will bring the collection to ⟨n⟩ items, above the recommended limit of ⟨ceiling⟩. You can still import. | Import; Cancel | 🤝 Aligned |
| E44 | Commit failed | Import didn’t finish | No room: There’s no room left where your file is. Free up space or move your file somewhere with room, then try again. Permission: The app can’t write to your file. Check its permissions, then try again. Unavailable location: The location of your file is unavailable. Reconnect it, then try again. Other: The app couldn’t save the import. All variants append: Nothing was imported; on a local volume your file is unchanged. | Try again; Cancel | 🤝 Aligned |
| E45 | Single-column read needs confirmation | This file was read as one column. Is that right? | Separator: ⟨delimiter⟩. If you expected more columns, choose the separator used by your spreadsheet. | Use this one column; Choose a separator; Cancel | 🤝 Aligned |

E43 renders the target and existing count as a first-class line, with encoding/delimiter controls always visible (R3.8n). Lists show names and source row numbers, with “None” for no added columns or exclusions; zero numeric counts remain visible. `⟨n⟩` in its warning is resulting collection size; `⟨ceiling⟩` is ROWS_CEILING. Other count placeholders name the corresponding R3.6 count; `⟨encoding⟩` and `⟨delimiter⟩` name the current read settings.

E44’s no-room variant carries [Data Foundation E15](../data-foundation/prd-data-foundation-copy.md#error--state-copy)’s cause and remedy; import retry always returns through a fresh preview (R3.8l). File movement uses the existing file-management surface after Cancel; the whole-or-nothing durability guarantee has [DF R1.7/R1.10](../data-foundation/prd-data-foundation.md#1-the-file-the-user-owns)’s local-volume scope.

## Generated column names

E12/E41 list each affected source position, original header (display “Unnamed” for blank), and final name. R2.5 owns collision order and R2.3 owns comparison; the naming strings are:

| Use | Template | Substitution |
| :--- | :--- | :--- |
| Blank header | `Column ⟨position⟩` | One-based source column position (for example, `Column 2`). |
| Collision | `⟨base⟩ (⟨suffix⟩)` | Original named header or generated blank name; suffix starts at 2 and increments until unique (for example, `Notes (2)`). |

The templates are part of saved header identity and must remain stable across reads. E12 and E41 may appear together; “Continue with the listed names” accepts all displayed resolutions (R3.8o).
