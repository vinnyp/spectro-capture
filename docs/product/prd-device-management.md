# PRD: Device Management

Author: Vinny Pasceri

# Background

SpectroCapture requires a connection to  a spectrophotometer to acquire color information. The scope of this PRD covers all the requirements for the use cases, features, and user journeys related to managing that connection. 

### Use cases

Defined in the [Vision doc](vision.md#use-cases):


| #   | Use case                                  | Persona     | What serves it                                                                   |
| --- | ----------------------------------------- | ----------- | -------------------------------------------------------------------------------- |
| U3  | **Start a session with a healthy device** | Cataloger   | known-device management (BLE + USB), calibration-due prompt, QR-tile calibration |
| U8  | **Scan where there is no internet**       | Cataloger   | offline-first design + per-device pre-authorization ("valid until ⟨date⟩")       |
| U9  | **Contribute code without hardware**      | Contributor | mock-device layer behind the device-service interface; what CI exercises         |


### Feature list

Defined in the [Vision doc](vision.md#feature-list):


| Release | Feature                                          | Serves | Notes                                                                               |
| ------- | ------------------------------------------------ | ------ | ----------------------------------------------------------------------------------- |
| v1      | Spectro 2/L connect (BLE + USB)                  | U3     |                                                                                     |
| v1      | Known-device management                          | U3     |                                                                                     |
| v1      | Tile calibration with due-prompts                | U3     |                                                                                     |
| v1      | Offline operation + per-device pre-authorization | U8     | "Authorized until 〈date〉" surfaced in the device panel                              |
| v1      | Mock-device layer                                | U9     | App runs, tests, and takes contributions with no hardware or key; what CI exercises |


## User Journeys

### UJ 1. First run

1. Install &amp; Open app
2. Initiate device discovery
3. Grant App bluetooth permissions
4. App detects a device is available to connect via USB or BLE
  - If no device detected → display a helpful message to turn on the device
5. If a NIX device is discovered, attempt to activate the NIX SDK license
  - If there's no internet connection → display a helpful message to connect to the internet
6. Initiate device pairing
  - If pairing fails → display a helpful error on how to recover
7. Initiate calibration
  - If calibration fails → display a helpful error on how to recover
8. App ready for acquisition

### UJ 1.1 Cannot complete a first run

1. Install &amp; Open app with no internet connection
2. Initiate device discovery
3. User denies App bluetooth permissions
  - Failure point: Permission is now denied, App can't detect the device via bluetooth
  - Display a helpful error message to enable bluetooth permissions
4. App can only detect device via USB
5. If a NIX device is discovered, attempt activation of the NIX SDK license
  - Failure point: No internet connect prevents the SDK from activating
  - Display a helpful error message to connect to the internet and allow the user to retry

### UJ 2. Start acquisition

1. Open app &amp; turn on device
2. Auto-reconnect to the last known connected device
3. Pre-flight health check (calibration, battery, authorization)
  - Optional calibration if gate tripped
  - Optional SDK authorization if tripped: date outside the granted window
4. Initiate acquisition
5. App ready for acquisition

### UJ 3. Remove a saved device

1. Initiate manage device
2. Select device → Remove

### UJ 4. Calibrate a connected device

1. Turn on device
2. App establishes connection to device
  - If connection isn't established → display a helpful error on how to recover
3. Initiate calibration
  - If calibration fails → display a helpful error on how to recover
4. App ready for acquisition

## Requirements

### Legend

**Priority**

- **P0:** Must have in this release
- **P1:** Should have in this release
- **P2:** Could have in this release

**Status**

- ⌛️ Ready for Alignment - Waiting for cross-functional team to align on requirements
- ✋ Needs Discussion - Cross-functional team needs to discuss with PM
- 🤝 Aligned - Cross-functional team aligned on the requirement
- 🦺 In Progress - Implementation in flight (Optional status)
- ✅ Completed - Implementation completed &amp; merged. PR # &amp; link added in the "Commit PR" column.
- ✂️ Deferred - Deferred from current release



### 1. Device Pairing

#### As a Cataloger, I can connect  a new spectrophotometer so that I can acquire color information into my collection.


| Release | Pri | Requirements                                                                                                                                                                                                                                              | Status                 | Commit PR |
| :------- | :--- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :---------------------- | :--------- |
| v1      | P0  | User must explicitly invoke an action to add a new device                                                                                                                                                                                                 | ⌛️ Ready for Alignment |           |
| v1      | P0  | User can connect to a Nix Spectro 2 or Spectro L                                                                                                                                                                                                          | ⌛️ Ready for Alignment |           |
| v1      | P0  | App must be able to discover a device via USB or BLE                                                                                                                                                                                                      | ⌛️ Ready for Alignment |           |
| v1      | P0  | For NIX devices, user must activate license key. If one isn't embedded into the SDK build, then the user can copy &amp; paste the key directly into the app to authorize the use of the SDK. User will be informed the activation will last until x date. | ⌛️ Ready for Alignment |           |
| v1      | P0  | If the app doesn't automatically discover the device, display a helpful warning indicating the device wasn't found, with the option to retry.                                                                                                             | ⌛️ Ready for Alignment |           |
| v1      | P0  | The list of discovered devices should be de-duped by the unique device ID, so a device that has been detected via USB and also BLE will only be listed once in the device list.                                                                           | ⌛️ Ready for Alignment |           |
| v1      | P0  | App must show a friendly name for the device. If there's a unique device id, that can be appended as well (e.g. "Spectro 2 (19H1)")                                                                                                                       | ⌛️ Ready for Alignment |           |


#### As a Cataloger, I can connect to a spectrophotometer I've used before so that I can quickly resume acquiring colors to my collection


| Release | Pri | Requirements | Status                 | Commit PR |
| :------- | :--- | :------------ | :---------------------- | :--------- |
| v1      | P0  |              | ⌛️ Ready for Alignment |           |
|         |     |              |                        |           |


