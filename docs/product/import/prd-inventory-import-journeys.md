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
| Every import state E4–E14, E38, E40–E45 (each variant) | Exercise every offered action, separately including Cancel | Destinations match R3.8; only E43 Import writes, Cancel preserves target, re-pick always re-reads; exclusions persist through Continue | R3.8a–o, R4.1–R4.2 |

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
| E40 | Go to session; cancel import; cancel ending; confirm ending (separate cases) | Open session/resume surface; exit unchanged; stay blocked; or follow Capture ending rules and return through fresh preview, respectively | R3.8 |
| Captured row with old name and nonblank metadata; source changes name and clears metadata | Leave E14’s “Take the new details” default untouched; import | Row updated, new name stored and metadata cleared, measurements/history unchanged | R3.5, R3.6c |
| Pending and set-aside items with code `CG 3`; separate fixtures import `cg  3` | Preview and commit | Same item, updated count, incoming displayed spelling; no E14, no new item, no state/order/measurement changes | R3.6f–g |
| Captured keep-row with a present blank field and an absent field; incoming values nonblank | Import | Present blank stays blank; absent field receives value; row unchanged unless another previously present field changes | R3.6d–g |
| 200 matched rows; source adds a new `Batch` column, all values `B1`, no other changes | Preview and import | Unchanged 200, no updated/new line; added-columns lists Batch; E43 says 200 rows gain details; all values stored as B1; repeat import adds nothing and omits the filled line | R3.6d/g/j, R3.9; E43 |
| Existing `Batch` column has B1 for 5 of 200 matched rows and is absent on 195; source supplies B1 for all, no other changes | Preview and import | Unchanged 200, no updated/new or added-columns line; E43 says 195 rows gain details; all 200 store B1; repeat import omits the filled line | R3.6d/g/j, R3.9; E43 |
| Only unchanged values supplied | Preview and import | Eligible all-unchanged import finishes without data changes; E43 has unchanged count and no filled/added-columns line | R3.6g/j, R3.9 |
| New empty target or existing target; preview has zero and nonzero counts (separate cases) | Review E43 | Target name always visible; zero-count sentences and empty list lines absent, remaining counts visible; no “None” placeholders | R3.1; E43; F63 |
| Stored `Notes` column and value; source ` notes ` in a new position and corrected value | Preview and import twice | One stored `Notes` column at its old position, corrected decoded value, no column addition; second import unchanged | R2.6, R3.6 |
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
