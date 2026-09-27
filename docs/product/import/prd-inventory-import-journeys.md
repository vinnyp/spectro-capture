# Inventory Import PRD — user journeys

Acceptance scenarios for [the PRD](prd-inventory-import.md); requirement rows own behavior and [the copy table](prd-inventory-import-copy.md#error--state-copy) owns strings. IDs and heading anchors survive the conversion from narrative journeys.

## User Journeys

Run these cases without hardware or a vendor credential (R4.1/R4.2). Compare store contents before and after; assert state/action identities independently of shipping text. In example strings, `\t`, `\n` and `\uXXXX` denote actual characters.

### Cluster 2 — Import an inventory

### UJ 2. Full collection bootstrap via CSV import

| Initial state | Action | Expected result | Rules / states |
| :--- | :--- | :--- | :--- |
| Readable CSV; no target yet | Pick or drop file; create target with name, sample count, scan mode and optional display defaults; map code; preview; commit | New rows pending in source order; no readings or session; queue survives relaunch | R1.1, R2.1–R2.2, R3.1–R3.3; [Capture R1.9](../capture-mode/prd-capture-mode.md#1-collections), [DF R1.1/R1.10](../data-foundation/prd-data-foundation.md#1-the-file-the-user-owns) |
| Same CSV encoded as UTF-8, with and without BOM | Use default settings | Same decoded headers and fields; BOM absent from first header | R1.5 |
| Valid UTF-16LE/BE or Windows-1252 input; semicolon or tab delimiter | Select the matching encoding and delimiter | Exact decoded field values; regenerated mapping/preview | R1.2, R1.5 |
| CSV includes `001`, `1/2`, a quoted comma, doubled quote, quoted newline, trailing empty field; LF or CRLF record endings | Read and import | All decoded text preserved; multiline field counts as one record; no extra record for terminal line ending | R1.4–R1.5 |
| Empty source | Pick file | E6; pick another file or cancel; no imported writes | R1.3, R1.5d, R3.8b |
| All-blank proposed header beside data records | Pick file | E4; choose a header or supply names; no imported writes | R1.3, R1.5d, R3.8a |
| Headerless file, or preamble before actual header | Supply names, or select the header record | All records are data in the first case: a wrong-width record 1 is listed as 1; with a header at record 3 after a preamble, a wrong-width record 5 is listed as 5 | R1.4–R1.5 |
| Invalid encoding bytes, contradictory BOM, or unclosed quoted field | Read file | E5 with encoding/syntax variant; no replacement bytes or guessed fields, no writes | R1.3, R1.5 |
| Header only | Read file | E6, pick another file or cancel; no import | R1.3, R3.8 |
| Wrong-width record plus valid records, one containing a quoted newline | Continue without excluded rows | E7 lists source record numbers; valid rows still pass through preview before commit | R1.4, R3.8 |
| All records excluded after validation | Attempt to proceed | E42 lists causes; commit unavailable; re-pick or cancel | R3.8–R3.9 |
| Valid preview; source changes, including a header change | Commit | E13; re-read, revalidate mapping, reset choices, and require a fresh preview | R3.1, R3.8 |
| Valid preview; source moved or removed | Commit | E38, re-pick or cancel; no imported writes | R1.3, R3.8 |
| Target created explicitly during import | Cancel before commit or inject commit failure | Created collection remains empty; no partial items or imported columns | R2.1, R3.2, R3.7 |
| Existing target; import would exceed ROWS_CEILING | Preview then commit | E43 names target, existing count, encoding/delimiter, nonzero outcome/filled counts, nonempty added-column and exclusion lists, and nonzero absent rows; warning names resulting size and ceiling; eligible rows still import | R3.1 |
| Semicolon or tab file, read with default comma | Attempt to save/reuse mapping | E45 blocks save/reuse; delimiter control available; choosing the correct separator re-reads into multiple columns and a fresh mapping/preview | R1.2, R1.6, R3.8a/m/n |
| One-column parse; no mapping saved | Map the single column to Swatch Code and attempt Import | E45 required before E43; commit unavailable until “Use this one column” | R1.6, R3.8k/m |
| Genuine one-column inventory | Choose “Use this one column”; map code; preview | Valid mapping can be saved/reused only after confirmation; E43 displays settings; a re-read requires confirmation again | R1.6, R3.8m/n |
| E5 for a UTF-16LE file with BOM | Choose UTF-8 (contradictory BOM), then UTF-16LE and the correct separator | Invalid retry remains E5 without writes; correct retry recovers through guard/mapping to E43 | R1.5a, R3.8a |
| Each available encoding fails for the same source and delimiter | Fail the default UTF-8 read, then manually select each remaining encoding | E5 retains its ordinary failure variant until every listed encoding has been attempted and failed; then its all-encodings-failed variant tells how to re-save as UTF-8; re-pick a valid export reaches preview; Cancel exits unchanged | R1.5a, R3.8b/j |
| Preview has captured matches with per-row and all-rows keep choices | Use E43’s “Choose an encoding” or “Choose a separator” (separate cases) | Re-read and revalidate mapping; one-column confirmation invalidated; fresh preview uses “Take the new details” default for every captured match | R1.5, R3.8n |
| Every import state E4–E14, E38, E40–E49 (each variant) | Exercise every offered action, separately including Cancel | Destinations match R3.8; only E43 Import writes, Cancel preserves target, re-pick always re-reads; exclusions persist through Continue | R3.8a–q, R4.1–R4.2 |

### UJ 2.1 Import additional rows into an existing collection

| Initial state | Action | Expected result | Rules / states |
| :--- | :--- | :--- | :--- |
| Existing pending, captured and set-aside rows; file contains matched, new and absent items | Import into existing target | No additional target-confirmation dialog; E43 names the target and its existing item count and counts all dispositions; only new rows append, matches retain state/order/measurements, absent items unchanged | R2.1, R3.1–R3.3 |
| Successful import | Repeat with identical mapping/choices and no intervening edits | All eligible rows unchanged; no duplicate items, column additions, queue changes or measurement writes | R3.3, R3.6–R3.7 |
| Stored metadata `Blue`; source blank, same value, changed value, or column omitted (separate cases) | Preview and commit | Blank clears; same is unchanged; changed replaces; omitted preserves; exercise mapped fields and passthrough metadata | R3.6 |
| Stored field already blank | Re-import blank field | Unchanged count, no proposed clear | R3.6 |
| Captured item has changed metadata, including a blank and differently spelled but equivalent Swatch Code | Toggle overwrite/keep for one row and all rows | E14; counts reflect chosen effects; keep preserves previously present values and fills absent fields; overwrite replaces supplied metadata; both preserve measurements/history | R3.5–R3.6 |
| Pending or set-aside item with changed metadata | Commit | Supplied changes applied without captured-row choice; state unchanged | R3.3, R3.5 |
| Existing metadata columns; file has a new column | Import twice | New column appends once; existing positions retained; R2.3-equal header reuses it on second import with first-seen spelling | R2.2, R3.6 |
| All captured matches choose keep; source adds a metadata column | Preview and commit | Added column listed separately from unchanged-row count; column appended and incoming values stored for each keep-row, even empty; previously present values unchanged | R3.5–R3.6 |
| One code matches two existing items | Continue without it | E11 changes neither item; both matches count as present, not absent, in E43; other eligible records may commit after preview | R3.4, R3.8 |
| Source contains two equivalent codes matching an existing item | Continue without duplicates | Both source records excluded; existing item unchanged and not counted as absent from source | R2.4, R3.6 |
| Existing target on a local volume | Inject no room, no permission, unavailable location, and other write failure (four cases); attempt import | E44 exposes the corresponding cause variant (no room uses DF E15); entire import rolled back on a local volume, including new columns and metadata clears; “Try again” requires a fresh preview, then succeeds once the fault is removed | R3.2 |
| Target session active, paused or interrupted (each case), or becomes active after preview | Select target / attempt commit | E40 names session, offers Go to the session / End that session / Cancel; no imported writes | R3.2, R4.1 |
| No session on the target; another collection's session active, paused or halted (each case) | Attempt commit | E40's another-collection variant names that collection, offers Go to the session / End that session / Cancel; no imported writes; once that session ends, a fresh preview commits (F66) | R3.2, R4.1 |
| No session anywhere; a [Collection Mode R8.1f/g write](../collection-mode/prd-collection-mode.md#8-operating-envelope-and-quality-attributes) on another collection, or a Data Foundation move, running, held until released ([Collection Mode R8.10a](../collection-mode/prd-collection-mode.md#8-operating-envelope-and-quality-attributes)) | Attempt commit while it runs; then again once it lands | While it runs the commit shows disabled and nothing is imported; once it lands the commit is offered (F66, F67) | R3.2, R4.1 |
| No session anywhere; this import's commit held running ([Collection Mode R8.10a](../collection-mode/prd-collection-mode.md#8-operating-envelope-and-quality-attributes)) | Try a capture start on another collection, Collection Mode's Rename collection and the Data Foundation re-read while it commits; then again once it lands | While it commits each shows disabled and nothing starts or is written; once it lands each is offered (F67) | R3.2, R4.1 |
| The target holds an interrupted session (E40's target variant); a [Collection Mode R8.1f/g write](../collection-mode/prd-collection-mode.md#8-operating-envelope-and-quality-attributes) on another collection held running until released ([Collection Mode R8.10a](../collection-mode/prd-collection-mode.md#8-operating-envelope-and-quality-attributes)) | While it runs, fire E40's End that session; then again once it lands | While it runs End that session shows disabled and no session ends; once it lands it is offered, and confirming it follows Capture's ending path (F67, F68) | R3.8, R4.1 |
| E40 | Go to session; cancel import; cancel ending; confirm ending (separate cases) | Open session/resume surface; exit unchanged; stay blocked; or follow Capture ending rules and return through fresh preview, respectively | R3.8 |
| Captured row with old name and nonblank metadata; source changes name and clears metadata | Leave E14’s “Take the new details” default untouched; import | Row updated, new name stored and metadata cleared, measurements/history unchanged | R3.5, R3.6c |
| Pending and set-aside items with code `CG 3`; separate fixtures import `cg  3` | Preview and commit | Same item, updated count, incoming displayed spelling; no E14, no new item, no state/order/measurement changes | R3.6f–g |
| Captured keep-row with a present blank field and an absent field; incoming values nonblank | Import | Present blank stays blank; absent field receives value; row unchanged unless another previously present field changes | R3.6d–g |
| 200 matched rows; source adds a new `Batch` column, all values `B1`, no other changes | Preview and import | Unchanged 200, no updated/new line; added-columns lists Batch; E43 says 200 rows gain details; all values stored as B1; repeat import adds nothing and omits the filled line | R3.6d/g/j, R3.9; E43 |
| Existing `Batch` column has B1 for 5 of 200 matched rows and is absent on 195; source supplies B1 for all, no other changes | Preview and import | Unchanged 200, no updated/new or added-columns line; E43 says 195 rows gain details; all 200 store B1; repeat import omits the filled line | R3.6d/g/j, R3.9; E43 |
| Only unchanged values supplied | Preview and import | Eligible all-unchanged import finishes without data changes; E43 has unchanged count and no filled/added-columns line | R3.6g/j, R3.9 |
| New empty target or existing target; preview has zero and nonzero counts (separate cases) | Review E43 | Target name always visible; zero-count sentences and empty list lines absent, remaining counts visible; no “None” placeholders | R3.1; E43; F63 |
| Stored `Notes` column and value; source ` notes ` in a new position and corrected value | Preview and import twice | One stored `Notes` column at its old position, corrected decoded value, no column addition; second import unchanged | R2.6, R3.6 |
| Stored column first seen as `Finish`, renamed `Surface` in Collection Mode ([its R4.8](../collection-mode/prd-collection-mode.md#4-item-detail-and-editing)); pending item A-1 holds `Matte` there | Import a source whose `surface` column holds `Gloss` for A-1 and whose `Finish` column holds `Satin` | `surface` reuses the stored `Surface` column at its position and A-1's value there becomes `Gloss`; `Finish` is listed as an added column and appends after the existing ones, holding `Satin` for A-1 (F65) | R2.6, R3.6 |
| Stored metadata `Blue`; source `blue` or ` Blue ` | Import with default choice | Exact text difference counts as updated even though R2.3 would match; store incoming text | R3.6 |

### UJ 2.2 Mapping metadata fields

| Initial state | Action | Expected result | Rules / states |
| :--- | :--- | :--- | :--- |
| No code mapping | Try to save mapping | E8; cannot save until one source maps to Swatch Code | R2.2 |
| Code, name, alternate code/name and extra columns | Map fields | Optional fields remain optional; each source/target used at most once; extra columns retained | R2.2 |
| Saved mapping; named headers reordered or case/spacing changed | Pick next file | Same signature restores identity mapping by name, never old position; R2.3-equal passthrough headers reuse the stored column and retain first-seen spelling and position | R1.2, R2.2, R2.6 |
| Blank header at position 2; explicit `Column 2` and `Column 2 (2)` elsewhere | Read file | Blank becomes `Column 2 (3)`; explicit names preserved; E12 lists generated name and position; “Continue with the listed names” reaches mapping, or re-pick reads fresh | R2.5 |
| Named headers `Name` and ` name ` | Read file | E41 lists both positions and resolved `Name` / ` name  (2)`; accept names before mapping, or re-pick; same source resolves identically on re-import | R2.5 |
| Whitespace-only code | Validate | E9 excludes record | R2.4 |
| Two codes equal in the table below | Validate | E10 excludes every member of the group; no winner | R2.3–R2.4 |
| Headers `Notes`, ` notes `, `Notes (2)`, plus a blank header | Accept all E12/E41 names | First spelling preserved; duplicate resolves to ` notes  (3)` because suffix 2 collides with a reserved input name; blank gets its positional name; stable mapping on repeat | R2.5, R3.8o |
| Stored column `Notes (2)` was explicitly named in one fixture and generated by a collision in another | Import `Notes`, `Notes` with distinct metadata values, then repeat | Both fixtures reuse stored `Notes (2)` for the second source column and `Notes` for the first; no provenance check, stored spelling/order retained, repeat adds no column | R2.5–R2.6 |
| Two codes unequal in the table below; empty target | Map, preview and import | Two eligible new items, no E10; exact display text retained | R2.3, R3.3a |


**R2.3 equality cases** — assert both equality and preserved display text. The same comparison serves collection names, headers, and Capture's find/duplicate checks.

| Left | Right | Equal? |
| :--- | :--- | :--- |
| ` CG\t3 ` | `cg\u00A0 3` | yes |
| `Caf\u00E9` | `CAFE\u0301` | yes |
| `Straße` | `STRASSE` | yes |
| `I` | `i` | yes |
| `I` | `ı` | no |
| `İ` | `i` | no |
| `café` | `cafe` | no |
| `Ａ` (fullwidth A) | `A` | no |
| `001` | `1` | no |

### Cluster 3 — Import a Nix Toolkit export

### UJ 3. Import a Nix Toolkit export with its readings

Every case uses a **synthetic Toolkit fixture** (R6.10): every collection name, code, name, note, date and value is invented, none taken from a real export. The format, from the one export studied (OQ 3): UTF-8 with LF record endings and no byte-order mark; fields separated by a bare `;` with no padding; an unquoted header of the Toolkit's 59 columns in order — `Index;Custom Collection Name;Color Name;Color Code;Nix Device;Note;Date Saved;Illuminant;Observer;Measurement Mode;L;a;b;L;c;h;X;Y;Z;sRGB R;sRGB G;sRGB B;HEX;Density Status;Density C;Density M;Density Y;Density K;R400 nm` … `R700 nm` at every 10 nm; Custom Collection Name, Color Name, Color Code and Note double-quoted on every data record and no other field quoted; Index a whole number; Date Saved in the form `YYYY-MM-DDTHH:MM:SS.sssZ`; the Lab, LCh, XYZ and reflectance cells in scientific notation with one integer digit, eight fraction digits and a one-digit signed exponent (for example `6.51234567e-1`); XYZ on a 0–1 scale; sRGB R, G and B whole numbers 0–255; HEX a `#` and six lowercase hex digits; Density Status one letter and the four densities two-place decimals that may be negative. The file's own Lab, LCh, XYZ, sRGB and HEX are checked in from an independent reference — a colour library at a stated version, or [Data Foundation R7.5](../data-foundation/prd-data-foundation.md#7-verifiability)'s published source once named — worked out from each record's reflectances under its illuminant and observer, never from the app's derivation.

**Fixture T** holds three records, Index 1–3, each with Custom Collection Name `Fixture Set`, Nix Device `Nix Spectro 2`, Note `undefined`, Illuminant `D50`, Observer `2`, Measurement Mode `M2`, Density Status `T` and invented densities: TK-1 (Color Code `TK-1`, Color Name `Pale Peach`, Date Saved `2026-03-14T09:15:00.000Z`, a spectrum whose colour sits inside sRGB), TK-2 (`TK-2`, `Deep Coral`, `2026-05-02T16:40:10.250Z`, a spectrum whose colour falls outside sRGB) and TK-3 (`TK-3`, `Sample Rust`, `2026-05-02T16:41:05.500Z`, one reflectance `1.04700000e+0`). Store contents are read by SQL at SQLITE_READER_FLOOR after the commit ([Data Foundation R7.1](../data-foundation/prd-data-foundation.md#7-verifiability)).

| Initial state | Action | Expected result | Rules / states |
| :--- | :--- | :--- | :--- |
| Fixture T; no target yet | Pick the file with default read settings | No E45: the file is read as a Toolkit export with the semicolon delimiter, encoding and separator shown fixed; E48 shows the fixed mapping, Color Code → Swatch Code and Color Name → Swatch Name, not editable; no E41 or E12 for the two L headers | R6.1, R6.2; E48 |
| Fixture T saved again with commas, every field otherwise as before | Pick the file | Read as a Toolkit export with the comma delimiter, reaching E48 as above | R6.1 |
| Fixture T without its `R550 nm` column | Pick the file | Not a Toolkit export: §1–§3's comma default applies and E45 asks to confirm one column | R6.1 |
| Fixture T with an extra last column `Extra` holding a value per record | Pick the file, continue, preview, Import | A Toolkit export; E43 lists `Extra` among added columns; after commit each item holds its `Extra` value as metadata | R6.1, R6.2 |
| Fixture T; no target yet | Create a target | Its name is pre-filled `Fixture Set`, editable; its scan mode is M2 and can't be changed in this creation | R6.3, R6.4 |
| Fixture T; a collection named `Fixture Set` already exists | Create a target, keeping the pre-filled name | [Capture E1](../capture-mode/prd-capture-mode-copy.md#error--state-copy)'s duplicate-name state blocks creation until the name is changed | R6.3 |
| Fixture T; an existing M2 target named `Other`, holding no item | Choose it, preview, Import | The target is still named `Other` | R6.3 |
| Fixture T; new target `Fixture Set` | Preview, then Import | E43's Toolkit lines lead: 3 readings measured in M2, 3 used as the swatch's colour, and no kept, already-here, mode-change, set-aside, differing, unchecked or same-date sentence; it offers neither "Choose an encoding" nor "Choose a separator"; after commit TK-1–TK-3 are captured, in source order, each carrying the imported mark ([Collection Mode R2.4j](../collection-mode/prd-collection-mode.md#2-the-collection-table-and-colour-honesty)) | R6.1, R6.8a, R6.9; E43 |
| The state the case above leaves | Read TK-1's, TK-2's and TK-3's current readings by SQL | TK-1's measurement time is `2026-03-14T09:15:00.000Z` and its record time the commit's; each reading's reflectances equal the fixture's as numbers, TK-3's `1.04700000e+0` stored as 1.047; samples, averaging basis, agreement verdict and spread not recorded; a snapshot of the imported kind, model `Nix Spectro 2`, serial and firmware empty; no raw payload, one never supplied; derived values under the collection's reference, D50/2°, M2 and the current derivation version, Lab within DERIVATION_TOLERANCE of the file's; TK-1's gamut-clipped flag clear and TK-2's set | R6.5; [Data Foundation R2.3j](../data-foundation/prd-data-foundation.md#2-canonical-value-and-version-history) |
| The same state | Read the collection's stored columns and TK-1's metadata by SQL | The imported columns are exactly Note, Density Status, Density C, Density M, Density Y and Density K; TK-1's densities hold the file's values; no item holds a Note; no column holds Custom Collection Name, Index, Nix Device, Date Saved, Illuminant, Observer, Measurement Mode, a reflectance, or the file's L, a, b, L, c, h, X, Y, Z, sRGB R, G, B or HEX | R6.2 |
| The same state, with TK-1's Note set to `Keep me` in Collection Mode | Import Fixture T again with TK-2's Note `Glossy` | TK-1's Note is still `Keep me` and E14 offers no change to it; TK-2's Note is `Glossy` | R6.2, R3.6 |
| Fixture T with TK-2's file L, a, b set 2 ΔE2000 away from its spectrum's | Preview | E43 lists TK-2 among readings whose colour values differ from the Toolkit's; after Import, TK-2's stored Lab is the one worked out from its spectrum | R6.6 |
| Fixture T with TK-1's file L, a, b set 0.05 ΔE2000 away from its spectrum's | Preview | TK-1 is not listed as differing | R6.6 |
| Fixture T with TK-3's file L empty, and TK-2's Illuminant `D65` and Observer `10`, its file values worked out under them | Preview, Import | E43 lists TK-2 and TK-3 as not checked and neither as differing; both import, their stored values under the collection's D50/2° | R6.6 |
| Fixture T with TK-3's `R550 nm` set to `abc` | Read the file | E47 lists record 4 and `R550 nm`; "Continue without them" previews TK-1 and TK-2 only | R6.7, R3.8c; E47 |
| Fixture T with TK-3's `R550 nm` `NaN`, TK-2's Date Saved `2026-05-02T16:40:10` with no zone, and TK-1's numbers written as plain decimals | Read the file | E47 lists record 4 with `R550 nm` and record 3 with `Date Saved`; TK-1 is eligible | R6.7; E47 |
| Fixture T with TK-2's Measurement Mode empty and TK-3's `M1` | Read the file | E47 lists record 3 with `Measurement Mode`; E46's mixed-modes variant then names M1 and M2 for the remaining records | R6.4, R6.7; E46, E47 |
| Fixture T with TK-2's Measurement Mode `M1` | Read the file | E46's mixed-modes variant names M1 and M2; "Pick the file again" reads fresh; no imported writes | R6.4, R3.8b; E46 |
| Fixture T with Custom Collection Name `Fixture Set` on TK-1 and `Other Set` on TK-2 | Read the file | E49 names Fixture Set and Other Set; "Pick the file again" reads fresh; no imported writes | R6.11, R3.8b; E49 |
| Fixture T; existing target using M1 that holds a captured item | Choose that target | E46's collection variant names M2 and M1; "Choose another collection" returns to target selection with the read kept; no imported writes | R6.4, R3.8p; E46 |
| Fixture T; existing target using M1 holding only pending items | Choose it, preview, Import | E43 says the collection will use M2, having used M1; after commit its scan mode is M2 and the readings land as R6.8a–b state | R6.4, R6.8, R6.9 |
| The same Given | Preview, then Cancel; separately, preview and Import with the commit failing (E44) | In both runs the target's scan mode is still M1 and nothing is imported | R6.4, R3.2, R3.8j |
| Fixture T; an existing M2 target holding only pending items | Preview; set the target's scan mode to M1 in its settings; Import | E46's collection variant; nothing is written | R6.4, R3.8k |
| Fixture T; an existing M2 target where TK-1 matches a captured item whose current live reading was measured `2026-02-01T08:00:00.000Z` and has a different Swatch Name, and TK-2 matches a pending item | Preview, keep E14's default, Import | E43 counts 2 used as colour (TK-2, TK-3) and 1 kept as earlier (TK-1); New 1 (TK-3), Updated 1 (TK-1); E14 shows its Toolkit variant; TK-1's scan stays current, its imported reading kept in history, not current, reason initial, its Swatch Name updated per R3.5; TK-2 is captured with its imported reading current and keeps its queue position | R6.8a–c, R6.9, R3.5; E14, E43 |
| The state the case above leaves | Import Fixture T again | E43 counts 3 already here and none used as colour or kept; TK-1 holds exactly two readings; nothing in the store changes | R6.8f, R6.9 |
| The state the first Import case leaves | Import Fixture T again | E43 counts 3 already here; rows Unchanged 3; no E14; nothing in the store changes | R6.8f, R6.9, R3.3 |
| The same state; Fixture T with TK-3's Date Saved `2026-06-01T10:00:00.000Z` and different reflectances, and TK-1's `2026-02-01T00:00:00.000Z` and different reflectances | Preview, Import | E43 counts 1 used as colour (TK-3), 1 kept as earlier (TK-1) and 1 already here (TK-2); TK-3's new reading is current, reason re-measurement, its first imported reading in history; TK-1's new reading is kept in history, not current, reason initial | R6.8d, R6.9 |
| The state the case above leaves | Import the original Fixture T again | E43 counts 3 already here; TK-1 and TK-3 each hold exactly two readings; nothing changes | R6.8f |
| The state the first Import case leaves; Fixture T with TK-2's reflectances changed and its Date Saved unchanged | Preview, Import | E43 lists TK-2 among same-dated readings kept as earlier; TK-2's current reading is unchanged and the new one is in history, not current, reason initial | R6.8d, R6.9 |
| The state the first Import case leaves, TK-2 then flagged as missing or damaged ([Capture R5.6](../capture-mode/prd-capture-mode.md#5-per-scan-failure-and-the-consecutive-failure-guard)), so set aside with no current value | Import Fixture T again | TK-2 stays set aside; E43 counts it already here; no reading is added | R6.8f |
| Fixture T; an existing M2 target where TK-1 is set aside, skipped with no reading, and TK-2's current reading is of the simulated kind ([Device R6.9](../device-management/prd-device-management.md#6-mock-device-layer)) | Preview, Import | E43 lists TK-1 among set-aside swatches given a colour; TK-1 is captured with its imported reading current, reason initial; TK-2's imported reading is current, reason re-measurement, its simulated reading in history | R6.8b, R6.8g, R6.9 |
| The state the first Import case leaves | Import Fixture T rewritten with every number as a plain decimal and each Date Saved with a `+00:00` offset | E43 counts 3 already here; nothing changes | R6.7, R6.8f |
| An existing M2 target holding TK-9, captured, and Fixture T's items as the first Import case left them | Import Fixture T | TK-9 is unchanged and counted as not in this file | R6.8e |
