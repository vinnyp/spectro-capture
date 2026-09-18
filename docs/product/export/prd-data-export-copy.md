# Data Export PRD — error & state copy

Companion to [prd-data-export.md](prd-data-export.md): the shipping copy for every state that PRD names.
Each `E<n>` state or `E<n><letter>` variant ID is the contract between a state and the rows that cite it; IDs never renumber. The Status column holds a value from that PRD's [Legend](prd-data-export.md#legend). A state whose name is marked ‹P1› does not appear until its own rows land, and a marked action or sentence inside an unmarked state is withheld until then — the P1-seam convention the sibling PRDs use. A marker leads the whole sentence it withholds and trails the whole action; it never sits inside one. The delete confirmations that offer an export first are the [Data Foundation PRD's E8 and E14](../data-foundation/prd-data-foundation-copy.md#error--state-copy)'s and are not restated here.

**Placeholders.** ⟨collection⟩ is the collection name or selected swatch code; ⟨kind⟩ is “current readings” or ‹P1› “full history”; ⟨items⟩ counts selected items and ⟨rows⟩ output rows. ⟨simulated⟩, ⟨non-spectral⟩, ⟨quarantined⟩, ⟨unscanned⟩ and ⟨unavailable⟩ are the distinct counts defined by [R1.1p](prd-data-export.md#preview-contract); ⟨condition⟩ is the chosen condition, ⟨renames⟩ the original → emitted header pairs, ⟨size⟩ a file size, ⟨path⟩ a location, ⟨format-version⟩ the export format version and ⟨app-version⟩ the writing app version. A named provisional constant renders as the number it currently holds, never its identifier.

Omit zero-count or absent-content clauses with their joining punctuation, and omit any line/action that cannot stand without them. Inflect counts (“1 reading”, “2 readings”); the explicit empty-collection variant below replaces the introductory count sentence at zero. Scope every disclosure to the selected kind before writing, and withhold P1 content until R1.3 lands.

## Error & state copy

Written in the Data consumer's vocabulary: plain language, names the recovery, never SDK-speak, never a schema word. Every promise below is backed by a requirement row in the PRD ([requirements](prd-data-export.md#requirements)).

| ID | State | Headline | Body | Primary action | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| E1 | Export a collection | Export ⟨collection⟩ | Export ⟨kind⟩: ⟨items⟩ swatches, ⟨rows⟩ rows.<br>The readings come out on ⟨condition⟩, the condition this collection is set to; readings with no wavelength data retain the reference they were measured under, which their qualifier columns identify; other condition sets stay in your collection file.<br>Available data includes your imported details, wavelength readings and six colour spaces — XYZ, Lab, LCh, Luv, sRGB and HSL — with the illuminant, observer, measurement condition and calculation version for each value. Missing values stay empty.<br>— Colours outside standard sRGB are marked.<br>— ⟨simulated⟩ readings came from the Demo Device.<br>— ⟨non-spectral⟩ readings have no wavelength data; the file says how their average was worked out.<br>— ⟨quarantined⟩ swatches have unreadable current readings; their colour and wavelength cells stay empty; archived instrument readings are included where available.<br>— ⟨unscanned⟩ swatches have not been scanned; their details still get a row.<br>— ⟨unavailable⟩ archived instrument readings are unavailable; those cells stay empty while intact colour and wavelength data still comes out.<br>— Renamed imported columns: ⟨renames⟩. Their values are unchanged.<br>Each reading includes its instrument serial and measurement time wherever those details are readable, even when its colour values cannot be read; archived instrument readings are included where available. Export kind: ⟨kind⟩. Export format: ⟨format-version⟩. SpectroCapture: ⟨app-version⟩. These versions are recorded on each exported data row. ‹P1› Full history includes every earlier reading as a separate row, with that reading’s measurement time. | Export current readings; Export with full history ‹P1›; Choose where | 🤝 Aligned |
| E2 | Export didn't finish — no room | There isn't room for this export | ⟨collection⟩ is untouched, nothing was written, and no half-finished file was left behind. ⟨path⟩ needs about ⟨size⟩ free. | Try again; Choose somewhere else | 🤝 Aligned |
| E3 | Export didn't finish — can't write there | That folder can't be written to | ⟨collection⟩ is untouched, nothing was written, and no half-finished file was left behind. Choose somewhere you can write to, or change who can write to ⟨path⟩. | Try again; Choose somewhere else | 🤝 Aligned |
| E4 | Export didn't finish — destination gone | That folder isn't there any more | ⟨collection⟩ is untouched, nothing was written, and no half-finished file was left behind. The drive may have been disconnected, or ⟨path⟩ moved. | Try again; Choose somewhere else | 🤝 Aligned |

## E1 variants

The contract is [R1.1h–l/s](prd-data-export.md#missing-data-matrix) and [R1.1o–r](prd-data-export.md#preview-contract), tested by [R4.4a](prd-data-export.md#surfaces); variants inherit E1’s status and phase rules; their wording implements those requirements.

| ID | Context | Rendering |
| :--- | :--- | :--- |
| E1a | Single item | Use its swatch code for ⟨collection⟩; counts include only that item and the selected kind’s rows |
| E1b | Canonical | ⟨kind⟩ = “current readings”; omit the redundant row count; measured-at includes readable time on quarantined current; the quarantine bullet counts affected swatches, not omitted history |
| E1c | History ‹P1› | ⟨kind⟩ = “full history”; counts cover all emitted reading rows plus never-scanned identity rows; omit row count when equal to item count; measured-at is each reading’s, not necessarily increasing; replace the quarantine bullet with “⟨quarantined⟩ readings cannot be read; their colour and wavelength cells stay empty; archived instrument readings are included where available.” |
| E1d | Empty collection | Replace the count sentence with “This collection has no swatches. The export contains column names only.”; omit condition/data/reading/count claims, the full-history sentence and “These versions are recorded on each exported data row.”; retain the export-kind/version disclosure and applicable actions |
| E1e | Missing content | List only available wavelength/colour-space categories; omit instrument/measurement-time/archive claims where none exists; the missing-values sentence remains when any values are absent |
| E1f | Unavailable archive | Count unavailable sample archives on the selected reading rows, excluding unused sample slots and samples that never supplied a payload; qualify archived-reading claims rather than promising complete payloads |
| E1g | Collision | List every original → emitted name, including literal `import_` names; do not limit the notice to original names starting with `sc_` |

Failure actions in E2–E4 are asserted independently of wording.
