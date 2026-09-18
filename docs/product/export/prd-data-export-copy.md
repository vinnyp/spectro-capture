# Data Export PRD — error & state copy

Companion to [prd-data-export.md](prd-data-export.md): the shipping copy for every state that PRD names.
Each `E<n>` ID is the contract between a state and the rows that cite it; IDs never renumber. The Status column holds a value from that PRD's [Legend](prd-data-export.md#legend). A state whose name is marked ‹P1› does not appear until its own rows land, and a marked action or sentence inside an unmarked state is withheld until then — the P1-seam convention the sibling PRDs use. A marker leads the whole sentence it withholds and trails the whole action; it never sits inside one. The delete confirmations that offer an export first are the [Data Foundation PRD's E8 and E14](../data-foundation/prd-data-foundation-copy.md#error--state-copy)'s and are not restated here.

**Placeholders.** ⟨collection⟩ is the collection name or selected swatch code; ⟨kind⟩ is “current readings” or ‹P1› “full history”; ⟨items⟩ counts selected items and ⟨rows⟩ output rows. ⟨simulated⟩, ⟨non-spectral⟩, ⟨quarantined⟩, ⟨unscanned⟩ and ⟨unavailable⟩ are the distinct counts defined by [R1.1p](prd-data-export.md#preview-contract); ⟨condition⟩ is the chosen condition, ⟨renames⟩ the original → emitted header pairs, ⟨size⟩ a file size and ⟨path⟩ a location. Never render a named constant as its identifier.

Omit zero-count or absent-content clauses with their joining punctuation, and omit any line/action that cannot stand without them. Inflect counts (“1 reading”, “2 readings”); the explicit empty-collection variant below replaces the introductory count sentence at zero. Scope every disclosure to the selected kind before writing, and withhold P1 content until R1.3 lands.

## Error & state copy

Written in the Data consumer's vocabulary: plain language, names the recovery, never SDK-speak, never a schema word. Every promise below is backed by a requirement row in the PRD ([requirements](prd-data-export.md#requirements)).

| ID | State | Headline | Body | Primary action | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| E1 | Export a collection | Export ⟨collection⟩ | Export ⟨kind⟩: ⟨items⟩ swatches, ⟨rows⟩ rows.<br>The readings come out on ⟨condition⟩, the condition this collection is set to; values worked out for other conditions stay in your collection file.<br>Available data includes your imported details, wavelength readings and six colour spaces — XYZ, Lab, LCh, Luv, sRGB and HSL — with the illuminant, observer and measurement condition for each value. Missing values stay empty.<br>— Colours outside standard sRGB are marked.<br>— ⟨simulated⟩ readings came from the Demo Device.<br>— ⟨non-spectral⟩ readings have no wavelength data; the file says how their average was worked out.<br>— ⟨quarantined⟩ readings cannot be read; their colour, wavelength and instrument-reading columns stay empty.<br>— ⟨unscanned⟩ swatches have not been scanned; their details still get a row.<br>— ⟨unavailable⟩ archived instrument readings are unavailable; those cells stay empty while intact colour and wavelength data still comes out.<br>— Renamed imported columns: ⟨renames⟩. Their values are unchanged.<br>Each available reading includes its instrument serial and when it was measured, plus the instrument’s own archived readings where available. The file says which kind of export it is, which version of the export format it is, and which version of SpectroCapture wrote it. ‹P1› Full history includes every earlier reading as a separate row, with that reading’s measurement time. | Export current readings; Export with full history ‹P1›; Choose where | 🤝 Aligned |
| E2 | Export didn't finish — no room | There isn't room for this export | ⟨collection⟩ is untouched, nothing was written, and no half-finished file was left behind. ⟨path⟩ needs about ⟨size⟩ free. | Try again; Choose somewhere else | 🤝 Aligned |
| E3 | Export didn't finish — can't write there | That folder can't be written to | ⟨collection⟩ is untouched, nothing was written, and no half-finished file was left behind. Choose somewhere you can write to, or change who can write to ⟨path⟩. | Try again; Choose somewhere else | 🤝 Aligned |
| E4 | Export didn't finish — destination gone | That folder isn't there any more | ⟨collection⟩ is untouched, nothing was written, and no half-finished file was left behind. The drive may have been disconnected, or ⟨path⟩ moved. | Try again; Choose somewhere else | 🤝 Aligned |

## E1 variants

The contract is [R1.1h–r](prd-data-export.md#missing-data-matrix), tested by [R4.4a](prd-data-export.md#surfaces); this table supplies wording/placeholder rules, not additional behavior.

| Context | Rendering |
| :--- | :--- |
| Single item | Use its swatch code for ⟨collection⟩; counts include only that item and the selected kind’s rows |
| Canonical | ⟨kind⟩ = “current readings”; measured-at means the current reading’s time, empty without a canonical value; count quarantined item rows, not damaged history omitted from this export |
| History ‹P1› | ⟨kind⟩ = “full history”; counts cover all emitted reading rows plus never-scanned identity rows; measured-at is each reading’s, which need not increase in row order |
| Empty collection | Replace the count sentence with “This collection has no swatches. The export contains column names only.”; omit condition/data/reading/count claims and the full-history sentence; retain the export-kind/version disclosure and applicable actions |
| Missing content | List only available wavelength/colour-space categories; omit instrument/measurement-time/archive claims where none exists; the missing-values sentence remains when any values are absent |
| Unavailable archive | Count unavailable sample archives on the selected reading rows, excluding unused sample slots and samples that never supplied a payload; qualify archived-reading claims rather than promising complete payloads |
| Collision | List every original → emitted name, including literal `import_` names; do not limit the notice to original names starting with `sc_` |

Quarantined history may retain readable provenance; it never supplies guessed device values or a current-reading claim. Failure actions in E2–E4 are asserted independently of wording.
