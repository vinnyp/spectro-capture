# Inventory Import PRD — user journeys

Acceptance scenarios for [the PRD](prd-inventory-import.md); requirement rows own behavior and [the copy table](prd-inventory-import-copy.md#error--state-copy) owns strings. IDs and heading anchors survive the conversion from narrative journeys.

## User Journeys

Run these cases without hardware or a vendor credential (R4.1/R4.2). Compare store contents before and after; assert state/action identities independently of shipping text. In example strings, `\t`, `\n` and `\uXXXX` denote actual characters.

### Cluster 2 — Import an inventory

### UJ 2. Full collection bootstrap via CSV import

| Initial state | Action | Expected result | Rules / states |
| :--- | :--- | :--- | :--- |
| Readable CSV; no target yet | Pick or drop file; create target with name, sample count, scan mode and optional display defaults; map code; preview; commit | New rows pending in source order; no readings or session; queue survives relaunch | R1.1, R2.1–R2.2, R3.1–R3.3 |
| Same CSV encoded as UTF-8, with and without BOM | Use default settings | Same decoded headers and fields; BOM absent from first header | R1.5 |
| Valid UTF-16LE/BE or Windows-1252 input; semicolon or tab delimiter | Select the matching encoding and delimiter | Exact decoded field values; regenerated mapping/preview | R1.2, R1.5 |
| CSV includes `001`, `1/2`, a quoted comma, doubled quote, quoted newline, trailing empty field; LF or CRLF record endings | Read and import | All decoded text preserved; multiline field counts as one record; no extra record for terminal line ending | R1.4–R1.5 |
| Empty source or all-blank proposed header | Pick file | E4; choose a header or supply names; no imported writes | R1.3, R1.5, R3.8 |
| Headerless file, or preamble before actual header | Supply names, or select the header record | All records are data in the first case; records preceding the chosen header are ignored in the second | R1.5 |
| Invalid encoding bytes, contradictory BOM, or unclosed quoted field | Read file | E5 with encoding/syntax variant; no replacement bytes or guessed fields, no writes | R1.3, R1.5 |
| Header only | Read file | E6, pick another file or cancel; no import | R1.3, R3.8 |
| Wrong-width record plus valid records, one containing a quoted newline | Continue without excluded rows | E7 lists source record numbers; valid rows still pass through preview before commit | R1.4, R3.8 |
| All records excluded after validation | Attempt to proceed | E42 lists causes; commit unavailable; re-pick or cancel | R3.7–R3.8 |
| Valid preview; source changes, including a header change | Commit | E13; re-read, revalidate mapping, reset choices, and require a fresh preview | R3.1, R3.8 |
| Valid preview; source moved or removed | Commit | E38, re-pick or cancel; no imported writes | R1.3, R3.8 |
| Target created explicitly during import | Cancel before commit or inject commit failure | Created collection remains empty; no partial items or imported columns | R2.1, R3.2, R3.7 |
| Existing target; import would exceed ROWS_CEILING | Preview then commit | Warning names resulting size; eligible rows still import | R3.1 |

### UJ 2.1 Import additional rows into an existing collection

| Initial state | Action | Expected result | Rules / states |
| :--- | :--- | :--- | :--- |
| Existing pending, captured and set-aside rows; file contains matched, new and absent items | Import into existing target | No additional target-confirmation dialog; preview counts all dispositions; only new rows append, matches retain state/order/measurements, absent items unchanged | R2.1, R3.1–R3.3 |
| Successful import | Repeat with identical mapping/choices and no intervening edits | All eligible rows unchanged; no duplicate items, column additions, queue changes or measurement writes | R3.3, R3.6–R3.7 |
| Stored metadata `Blue`; source blank, same value, changed value, or column omitted (separate cases) | Preview and commit | Blank clears; same is unchanged; changed replaces; omitted preserves; exercise mapped fields and passthrough metadata | R3.6 |
| Stored field already blank | Re-import blank field | Unchanged count, no proposed clear | R3.6 |
| Captured item has changed metadata, including a blank and differently spelled but equivalent Swatch Code | Toggle overwrite/keep for one row and all rows | E14; counts reflect chosen effects; keep preserves all values; overwrite replaces supplied metadata; both preserve measurements/history | R3.5–R3.6 |
| Pending or set-aside item with changed metadata | Commit | Supplied changes applied without captured-row choice; state unchanged | R3.3, R3.5 |
| Existing metadata columns; file has a new column | Import twice | New column appends once; existing positions retained; exact-name match reuses it on second import | R2.2, R3.6 |
| All captured matches choose keep; source adds a metadata column | Preview and commit | Added column listed separately from unchanged-row count; column appended; existing row values unchanged | R3.5–R3.6 |
| One code matches two existing items | Continue without it | E11 changes neither item; other eligible records may commit after preview | R3.4, R3.8 |
| Source contains two equivalent codes matching an existing item | Continue without duplicates | Both source records excluded; existing item unchanged and not counted as absent from source | R2.4, R3.6 |
| Existing target; store fails during commit | Attempt import | Entire import rolled back, including new columns and metadata clears | R3.2 |
| Target session active, paused or interrupted (each case), or becomes active after preview | Select target / attempt commit | E40 names session, offers Go to the session / End that session / Cancel; no imported writes | R3.2, R4.1 |
| E40 | Go to session; cancel import; cancel ending; confirm ending (separate cases) | Open session/resume surface; exit unchanged; stay blocked; or follow Capture ending rules and return through fresh preview, respectively | R3.8 |

### UJ 2.2 Mapping metadata fields

| Initial state | Action | Expected result | Rules / states |
| :--- | :--- | :--- | :--- |
| No code mapping | Try to save mapping | E8; cannot save until one source maps to Swatch Code | R2.2 |
| Code, name, alternate code/name and extra columns | Map fields | Optional fields remain optional; each source/target used at most once; extra columns retained | R2.2 |
| Saved mapping; named headers reordered or case/spacing changed | Pick next file | Same signature restores identity mapping by name, never old position; differently spelled passthrough names remain distinct stored columns | R1.2, R2.2 |
| Blank header at position 2; explicit `Column 2` and `Column 2 (2)` elsewhere | Read file | Blank becomes `Column 2 (3)`; explicit names preserved; E12 lists generated name and position | R2.5 |
| Named headers `Name` and ` name ` | Read file | E41 lists both positions; no saved mapping applied or saved; fix source and re-pick | R2.5 |
| Whitespace-only code | Validate | E9 excludes record | R2.4 |
| Two codes equal in the table below | Validate | E10 excludes every member of the group; no winner | R2.3–R2.4 |

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
