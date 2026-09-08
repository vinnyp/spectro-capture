# Inventory Import PRD — error & state copy

Companion to [prd-inventory-import.md](prd-inventory-import.md): the shipping copy for every state that PRD names.
Each `E<n>` ID is the contract between a state and the rows that cite it; IDs never renumber, and these thirteen kept the numbers they carried in the capture PRD when they moved here under fence F49. The Labels and Placeholders rules that govern this table are the [capture PRD's §12](../capture-mode/prd-capture-mode.md#12-error--state-copy)'s, and the Status column here holds a value from this PRD's [Legend](prd-inventory-import.md#legend).

## Error & state copy

The shipping copy for every error, waiting, choice, and confirmation state in this PRD, written in the Cataloger's vocabulary: plain language, names the recovery, never SDK-speak. Every state is a distinct named state whose identity is stable even when its wording changes, so behaviour can be asserted independently of copy ([R4.1](prd-inventory-import.md#4-demo-device-and-verifiability)). Every promise made below is backed by a requirement row in the PRD.

| ID | State | Headline | Body | Primary action | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| E4 | No header row | Which row has your column names? | This file doesn't seem to start with a row of column names. Pick the row that does, or name the columns yourself. | Pick the header row | 🤝 Aligned |
| E5 | Can't read the file | Can't read this file | The text in this file isn't in a form the app recognizes. Try saving it again from your spreadsheet as CSV, or choose the text encoding yourself. Nothing has been imported. | Choose an encoding; Pick a different file | 🤝 Aligned |
| E6 | Nothing to import | This file has column names but no rows | There's nothing to bring in. Check you picked the right file. | Pick a different file | 🤝 Aligned |
| E7 | Rows with the wrong number of columns | ⟨n⟩ rows don't line up with the columns | These rows have more or fewer values than the header row, so they're listed below and left out. You can bring in the rest, or fix the file and try again. | Import without them; Pick the file again | 🤝 Aligned |
| E8 | Mapping incomplete | Tell the app which column is the swatch code | Every swatch needs a code — it's how the app matches a row when you re-import, and how you find a swatch later. | Choose the code column | 🤝 Aligned |
| E9 | Blank codes excluded | ⟨n⟩ rows have no swatch code | Rows without a code are listed below and left out, because there'd be no way to find them again. Bring in the rest, or fill the codes in and try again. | Import without them; Pick the file again | 🤝 Aligned |
| E10 | Duplicate codes in the file | ⟨n⟩ rows share a swatch code | Every row listed below is left out — the app won't guess which one you meant. Spacing and capitals don't count as a difference, so "CG 3" and "cg  3" — the same letters with an extra space — are the same code. Bring in the rest, or fix the file and try again. | Import without them; Pick the file again | 🤝 Aligned |
| E11 | Ambiguous code | ⟨code⟩ matches more than one swatch in this collection | This swatch is left out rather than guessed at. Nothing in your collection changes. | Continue without it | 🤝 Aligned |
| E12 | Unnamed column | ⟨n⟩ columns have no name | They're coming in as "Column 7" and so on. You can rename them later when you're browsing the collection. | Continue | 🤝 Aligned |
| E13 | File changed on disk | This file changed while you were looking at it | The app has read it again — here's what it will bring in now. | Review again | 🤝 Aligned |
| E14 | Changed metadata on a captured row | ⟨n⟩ swatches you've already scanned have different details in this file | The app will take the new details, because a corrected spreadsheet is the usual reason to import again — change that here, for all of them or one at a time. Where the file leaves a field empty, taking the new details empties that field too; those are listed below. Your measurements aren't touched either way. | Take the new details; Keep what I have | 🤝 Aligned |
| E38 | File gone before the import | That file isn't where it was | It's been moved, renamed, or deleted since you picked it. Nothing has been imported. Pick it again and the app will read it fresh. | Pick the file again; Cancel | 🤝 Aligned |
| E40 | Import blocked by a session | Finish your session in ⟨collection⟩ first | A session is open here, so new swatches can't join its queue mid-run. End or finish the session, then import — nothing about the file has changed. | Go to the session; End that session; Cancel | 🤝 Aligned |
