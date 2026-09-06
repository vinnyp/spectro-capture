# PRD: Capture Mode

Author: Vinny Pasceri

# Background

SpectroCapture's primary value proposition is quick bulk capture of information using your spectrophotometer. The scope of this PRD covers all the requirements for the use cases, features, and user journeys related to the capture experience.

### Use cases

Defined in the [Vision doc](vision.md#use-cases):


|     | Use case                                 | Persona   | What serves it                                                         |
| --- | ---------------------------------------- | --------- | ---------------------------------------------------------------------- |
| U1  | **Bulk-digitize a predefined inventory** | Cataloger | CSV import with column mapping → queued scan, 1–5 samples/row averaged |
| U2  | **Capture a single new item ad hoc**     | Cataloger | metadata-first single capture into a chosen collection                 |


### Feature list

Defined in the [Vision doc](vision.md#feature-list):


| Pri | Feature                                  | Serves | Notes                                                          |
| --- | ---------------------------------------- | ------ | -------------------------------------------------------------- |
| P0  | CSV inventory import with column mapping | U1     | The inventory-first wedge                                      |
| P0  | Queued bulk scan, 1–5 samples averaged   | U1     | Heads-down; haptic confirm where available; row auto-advance   |
| P0  | Inline scan-failure handling             | U1     | Retry / skip / flag-row for light, battery, temperature errors |
| P1  | Ad-hoc single capture                    | U2     | Metadata-first, into a chosen collection                       |


## User Journeys

### UJ 1. Manual creation of a library &amp; first collection

1. Initiate action to create a library, which contains one or more collections
2. Name the Library
3. Initiate an action to create a collection within the library
4. Name the collection
5. Select the collection illuminant (e.g. D50)
6. Select the collection Observer (e.g. 2º, M1)
7. Select the number of samples to acquire (e.g. 3)
8. Save the collection
9. Start capture session

### UJ 1.1. Add a new collection

1. Open an existing library
2. Initiate an action to create a new collection within the library
3. Name the collection
4. Select the collection illuminant (e.g. D50)
5. Select the collection Observer (e.g. 2º, M1)
6. Select the number of samples to acquire (e.g. 3)
7. Save the collection
8. Start capture session

### UJ 1.2. Delete a collection

1. Open a library
2. Select a collection; initiate an action to delete the collection
3. Display a warning that the collection and all information contained within in will be deleted
4. If the user confirms, delete the collection and the collection data.

### UJ 1.3 Rename a collection

1. Open a library
2. Select a collection; initiate an action to rename the collection
3. Rename the collection to a new unique name
4. Save the new collection name

### UJ 1.4 Rename a Library

1. Select a library; initiate an action to rename a the library
2. Rename the library to a new unqiue name
3. Save the new library name

### UJ 1.5 Delete a Library

1. Select a library; initiate an action to delete the library
2. Display a warning indicating the library and all contents within will be deleted (all collections and data within those collections)
3. If the user confirms, delete the library and all data.

### UJ 1.6 Move a collection to another library

1. Select a library
2. Select a collection; initiate an action to move a collection to another library. If no other libraries exist, give users the ability to create a new library from this point.
3. Display a list of available libraries. If the UX allows for drag &amp; drop, allow the user to drag &amp; drop the collection to another library.
4. Once the user selects the library, prompt the user to confirm they would like to move the collection to another library.
5. If the user confirms, move the collection to the new library. Verify all the data has successfully moved as part of the move.

### UJ 2. Full collection bootstrap via CSV import

1. Initiate an action to import a collection via CSV
2. Select the CSV to import (or drag &amp; drop the CSV)
3. Map the column of the library name to the canonical library field. No library exists with that name, and a UX affordance indicates as such.
4. Map the column of the collection name to the canonical collection field. No collection exists with that name, and a UX affordance indicates as such.
5. Map the metadata fields to the canonical collection metadata fields
6. App validates the column mapping and allows the user to save
7. App indicates a new library and a new collection will be created and asks the user to validate
8. Save the mapping
9. Select the collection illuminant (e.g. D50)
10. Select the collection Observer (e.g. 2º, M1)
11. Select the number of samples to acquire (e.g. 3)
12. Save
13. Create the new library and collection
14. Import the data into the new collection
15. Start capture session

### UJ 2.1 Import CSV to existing Library

1. Initiate an action to import a collection via CSV
2. Select the CSV to import (or drag &amp; drop the CSV)
3. Map the column of the library name to the canonical library field. A library exists with that name, and a UX affordance indicates as such.
4. Map the column of the collection name to the canonical collection field. No collection exists with that name, and a UX affordance indicates as such.
5. Map the metadata fields to the canonical collection metadata fields
6. App validates the column mapping and allows the user to save
7. App indicates a new  collection will be created in the existing library and asks the user to validate
8. Save the mapping
9. Select the collection illuminant (e.g. D50)
10. Select the collection Observer (e.g. 2º, M1)
11. Select the number of samples to acquire (e.g. 3)
12. Save
13. Create the new collection in the existing library
14. Import the data into the new collection
15. Start capture session

### UJ 2.2 Import additional rows via to an existing collection

1. Initiate an action to import a collection via CSV
2. Select the CSV to import (or drag &amp; drop the CSV)
3. Map the column of the library name to the canonical library field. A library exists with that name, and a UX affordance indicates as such.
4. Map the column of the collection name to the canonical collection field. A collection exists with that name, and a UX affordance indicates as such.
5. Map the metadata fields to the canonical collection metadata fields
6. App validates the column mapping and allows the user to save
7. Save the mapping
8. App indicates there are existing rows in the collection and asks the user to verify to import new rows into that collection. Rows that are already found in the collection are not re-imported (i.e. idempotent).
9. Import the data into the new collection
10. Start capture session

### UJ 2.3 Mapping metadata fields

1. Assume the user is at the point they need to map metadata fields
2. Map the Swatch Code field
3. Map the Swatch Name field
4. Optional: Map the Swatch Alternate Code
5. Optional: Map the Swatch Alternate Name
6. Optional: For any additional columns that aren't mapped, they will be imported "as is" into the collection as additional metadata fields the collection experience can surface

### UJ 3. Initiating a bulk capture

1. Initiate a capture
2. If no library &amp; collection is selected, select a library → select a collection
3. Queue the next item to be captured
4. Initiate an action to acquire from device
5. Device acquires color information; loop until number of captures are met (e.g. 3 captures)
6. Save the color information
7. Queue the next item; continue until session completes

### UJ 4. Initiating an ad-hoc single capture

1. Initiate a capture
2. If no library &amp; collection is selected, select a library → select a collection
3. Queue the next item to be captured
4. Regardless of the next item, user initiates an action to add a new swatch
5. User enters Swatch Code, Swatch Name, and optionally adds Swatch Alternate Code, Swatch Alternate Name
6. Save
7. Initiate an action to acquire from device
8. Device acquires color information; loop until number of captures are met (e.g. 3 captures)
9. Save the color information
10. Queue the next item; continue until session completes

## Requirements

### Legend

**Priority**

Priority is build order within v1, not a cut line — everything in this document ships in v1: P0 is the first build phase, P1 the second, P2 last.

Provisional constants: every TBD-on-spike constant is a named provisional constant carrying its candidate value and its [Open Questions](#open-questions) id; mechanisms build against these constants, and no provisional constant ships in a release without its OQ resolved.

**Status**

- ⌛️ Ready for Alignment - Waiting for cross-functional team to align on requirements
- ✋ Needs Discussion - Cross-functional team needs to discuss with PM
- 🤝 Aligned - Cross-functional team aligned on the requirement
- 🦺 In Progress - Implementation in flight (Optional status)
- ✅ Completed - Implementation completed &amp; merged. PR # &amp; link added in the "Commit PR" column.
- ✂️ Deferred - Deferred from current release

