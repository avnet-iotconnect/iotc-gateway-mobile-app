# Engineering Note — SensorTile.box PRO Hardware Revisions, DATALOG2 Firmware & BlueST SDK Compatibility

This note documents **why the AIoT Craft labs standardize on FP-SNS-DATALOG2_Datalog2 v3.1.0**, how the SensorTile.box PRO identifies itself over Bluetooth, and which hardware, firmware, and mobile-app combinations are currently compatible. It supports the [ST AIoT Craft guide](./st_aiotcraft_guide.md), [Lab 1](./st_aiotcraft_lab1_deploy.md), and [Lab 2](./st_aiotcraft_lab2_train.md), and is **cloud-agnostic** — the same rules apply whether the Bridge App is signed into the AWS POC, production AWS, or an Azure /IOTCONNECT environment (see [Section 6](#6-cloud-environments-aws-poc-production-aws-azure)).

> **Last verified: July 2026.** The published SDK and catalog state described below will change as ST issues new releases; re-verify version-specific statements before relying on them.

## Summary of Operating Requirements

1. **All devices run FP-SNS-DATALOG2_Datalog2 v3.1.0**, regardless of hardware revision (A, B, or C). It is currently the only DATALOG2 release recognized by the Bridge App on **both** iOS and Android.
2. **Do not allow the ST AIoT Craft mobile application to update device firmware.** It installs v3.2.0, which the Bridge App on iOS cannot recognize. If a device has been updated, reflash v3.1.0 ([Section 7](#7-flashing-reference)).
3. **Device names must be exactly 7 characters, alphanumeric only.** The firmware stores names in a fixed 7-byte field without validation: shorter names are persisted with residual bytes appended (this corruption survives reflashing), and longer names overflow the in-memory buffer.
4. **Do not delete a device record in the /IOTCONNECT console.** The application registers a device only on first pairing and holds the auto-generated certificates; after a console-side deletion, the application enters a permanent reconnection cycle (approximately every 20 seconds). Recovery requires uninstalling and reinstalling the Bridge App, then pairing again.

## 1. Hardware Revisions

ST has shipped three revisions of the SensorTile.box PRO ([STEVAL-MKBOXPRO](https://www.st.com/en/evaluation-tools/steval-mkboxpro.html)). The label on the enclosure shows the order code:

| Revision | Order code | Hardware change | Sensor set |
|---|---|---|---|
| Rev A | STEVAL-MKBOXPRO**A** | Baseline (NFC: ST25DV04K, BLE: BlueNRG-LP) | LSM6DSV16X, LIS2DU12, LIS2MDL, LPS22DF, STTS22H, MP23DB01HP |
| Rev B | STEVAL-MKBOXPRO**B** | NFC IC changed to ST25DV04**KC** | identical |
| Rev C | STEVAL-MKBOXPRO**C** | BLE controller changed to STM32WB07 | identical |

The sensor complement — and therefore everything the labs exercise (MLC models, data logging, inference) — is **identical across revisions**. The revisions matter only because of how the firmware announces itself, described below.

## 2. Device Recognition — BlueST Identity and the Catalog

Per the [BlueST Protocol v2](https://staiotcraft.st.com/index.html#/wiki?id=4_5_bluest_protocol.md), the device advertises a **(board ID, firmware ID)** byte pair over BLE. Client applications look this pair up in ST's firmware catalog ([STMicroelectronics/appconfig](https://github.com/STMicroelectronics/appconfig), `bluestsdkv2/catalog.json`) to determine the device type and its capabilities. Two consequences follow:

- **A pair that is not present in the application's catalog results in an unrecognized device.** The application falls back to raw BLE feature names (`MachineLearningCoreFeature`, `HSDFeature`, `RawPnPLControlledFeature`) or fails in one of the modes described in [Section 5](#5-failure-signatures).
- **Applications do not read the live catalog.** Each build references a *frozen snapshot branch* (`blesensor_<version>`) — the "**catalog pin**". A stale pin silently prevents recognition of newer firmware.

The DATALOG2 firmware detects the hardware revision at boot (via the NFC IC) and selects its advertised identity accordingly. The mapping **changed at v3.2.0** — the separate Rev B identity was retired, and Rev B devices began reporting as Rev C:

| Hardware | Firmware | Advertises (board ID, fw ID) |
|---|---|---|
| Rev A | v3.1.0 | `0x0D` / `0x1D` |
| Rev B | v3.1.0 | `0x11` / `0x18` |
| Rev A | v3.2.0 / v3.3.0 | `0x0D` / `0x26` / `0x28` |
| Rev B **and** C | v3.2.0 / v3.3.0 | `0x13` / `0x08` / `0x0A` |

(Board IDs are official: `0x0D` = MKBOXPRO, `0x11` = MKBOXPROB, `0x13` = MKBOXPROC — see the Device Id table in the BlueST protocol wiki. Firmware detection source: [fp-sns-datalog2](https://github.com/STMicroelectronics/fp-sns-datalog2), `Projects/STM32U585AI-SensorTile.boxPro/Applications/DATALOG2/Core/Src/App.c` per tag.)

## 3. DATALOG2 Firmware Versions

| Version | Released | Box-PRO-relevant changes |
|---|---|---|
| **v3.1.0** | Jul 2025 | **Standard version for the labs.** Baseline for AIoT Craft. |
| v3.2.0 | Dec 2025 | DFU-entry fix; **identity change** (Rev B reports as Rev C). New sensor support targets STWIN.box / Nucleo shields, not the box PRO. |
| v3.3.0 | Jun 2026 | SD-close timeout fix; support for the IIS3DWB10IS **add-on module**. No changes to the onboard sensor set. |

**Versions 3.2.0 and 3.3.0 introduce no capability used by these labs.** The onboard sensors, MLC workflow, BLE streaming, and SD logging are unchanged. Adopting a later version therefore provides no benefit while breaking iOS recognition (see the following section) — this is the basis for standardizing on v3.1.0.

## 4. BlueST SDK Status (as of July 2026)

The Bridge App is built on ST's BlueST SDKs, whose published states diverge significantly by platform:

| | Android — [BlueSTSDK_Android](https://github.com/STMicroelectronics/BlueSTSDK_Android) | iOS — [BlueSTSDK_iOS](https://github.com/STMicroelectronics/BlueSTSDK_iOS) |
|---|---|---|
| Release cadence | Approximately every 2 months (V1.2.8 → V1.2.18, Dec 2024–Apr 2026) | 3 releases since 2023; latest **v1.1.1 (Nov 2025)** |
| Catalog pin | `blesensor_5.3.0` (contains all v3.2.0+ entries) | defaults to `blesensor_5.2.0` (contains **no** v3.2.0+ entries) |
| Rev C board type (`0x13`) | supported | supported only from v1.1.1 |
| AIoT Craft firmware allowlist | none found | hardcoded `["3.1.0"]` (`Firmware.swift`) |

The resulting compatibility matrix for the Bridge App:

| Firmware on device | Android | iOS |
|---|---|---|
| v3.1.0 (any revision) | ✅ | ✅ |
| v3.2.0 / v3.3.0 (any revision) | ✅ | ❌ |

Two additional considerations:

- **ST's AIoT Craft mobile application installs v3.2.0.** It is built from a newer internal SDK state that has not been published; third-party applications can only build from the public repositories, which predate every currently shipping DATALOG2 release. Resolution of the iOS limitation therefore depends on ST either publishing the updated SDK or sharing the application's SDK configuration.
- The current iOS application source published by ST ([STBLESensor_iOS](https://github.com/STMicroelectronics/STBLESensor_iOS)) pins catalog `5.3.0` via `CFBundleCatalogVersionString`, and serves as the reference implementation for any iOS application updating its pin.

## 5. Failure Signatures

Symptoms observed during lab preparation, with root cause and resolution:

| Symptom | Cause | Resolution |
|---|---|---|
| Selecting the device shows `MachineLearningCoreFeature`, `HSDFeature`, `RawPnPLControlledFeature`; no AI Model List | Unknown **board ID** — the device is on v3.2.0+ and the application's SDK/catalog predates its identity | Reflash v3.1.0 |
| Device connects and disconnects in a BLE loop on pairing (iOS) | Known board, unknown **firmware ID** — the application engages its device flow, cannot resolve the firmware, and retries indefinitely | Reflash v3.1.0 |
| Application shows "connecting to /IOTCONNECT… disconnected" approximately every 20 seconds and never connects | The device record was deleted in the console; the application retries against a registration that no longer exists (it holds the certificates, so console-side re-creation cannot restore the connection) | Uninstall and reinstall the Bridge App, sign in, pair again |
| Device advertises a corrupted name, or the cloud rejects the device ID after a rename | The firmware stores names as a fixed 7-byte copy without length validation | Rename with **exactly 7** alphanumeric characters |

The device name is stored in a dedicated flash page (`0x080FE000`) and **survives reflashing** — flashing firmware neither repairs nor corrupts a stored name.

## 6. Cloud Environments: AWS POC, Production AWS, Azure

Everything in Sections 1–5 occurs on the **BLE side of the bridge** and is **identical across cloud environments** — the device has no knowledge of which /IOTCONNECT instance the phone is signed into. The cloud half differs per environment:

| Environment | Console | Used by | AIoT Craft flow |
|---|---|---|---|
| AWS POC | `awspoc.iotconnect.io` | [AIoT Craft guide](./st_aiotcraft_guide.md) / [Lab 1](./st_aiotcraft_lab1_deploy.md) / [Lab 2](./st_aiotcraft_lab2_train.md) | ✅ (preview environment) |
| Production AWS | `console.iotconnect.io` | [Main mobile app guide](./mobile_app_guide.md) | Planned mid-summer |
| Azure | per-tenant /IOTCONNECT Azure instance | selectable in the Bridge App login | Not available (the AIoT Craft preview is AWS-POC-only) |

Environment-portability rules:

- **Device records, templates (`AvnetSTaws`), models, and dashboards are scoped to a single environment and account.** No artifacts migrate when the application's login environment changes; pairing a device while signed into a different environment creates a new registration there.
- The firmware version requirement (v3.1.0) and naming requirement (7 characters) apply **unchanged** in every environment, because recognition occurs before the cloud is involved.
- The [failure signatures](#5-failure-signatures) table applies unchanged in every environment for the same reason — with one addition: the deleted-device-record condition is also per-environment, so a device may operate normally in one environment while failing to connect in another.

## 7. Flashing Reference

1. Enter DFU mode: hold the BOOT button while connecting USB-C ([UM3133 §2.2](https://www.st.com/resource/en/user_manual/um3133-getting-started-with-sensortilebox-pro-multisensors-and-wireless-connectivity-development-kit-for-any-intelligent-iot-node-stmicroelectronics.pdf)). The board enumerates as "STM32 BOOTLOADER".
2. Flash the stock v3.1.0 binary — shipped by ST inside the [fp-sns-datalog2 v3.1.0 tag](https://github.com/STMicroelectronics/fp-sns-datalog2/tree/v3.1.0/Projects/STM32U585AI-SensorTile.boxPro/Applications/DATALOG2/Binary) (`DATALOG2_Release.bin`) — at address `0x08000000` with [STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html):

   ```
   STM32_Programmer_CLI -c port=usb1 -w DATALOG2_Release.bin 0x08000000 -v -rst
   ```

3. The device's 7-character name survives the reflash (it is stored outside the firmware image). Verify that the device reappears in the Bridge App under its printed identifier.

## Sources

* [fp-sns-datalog2](https://github.com/STMicroelectronics/fp-sns-datalog2) — firmware source, tags v3.1.0–v3.3.0 (identity logic: `App.c`, ID values: `sysconfig.h`)
* [appconfig](https://github.com/STMicroelectronics/appconfig) — the BlueST firmware catalog; snapshot branches `blesensor_*`
* [BlueSTSDK_Android](https://github.com/STMicroelectronics/BlueSTSDK_Android) / [BlueSTSDK_iOS](https://github.com/STMicroelectronics/BlueSTSDK_iOS) / [STBLESensor_iOS](https://github.com/STMicroelectronics/STBLESensor_iOS) — SDK sources, catalog pins, allowlist
* [ST AIoT Craft wiki — BlueST Protocol](https://staiotcraft.st.com/index.html#/wiki?id=4_5_bluest_protocol.md) and [Default Sensor Node Firmware](https://staiotcraft.st.com/index.html#/wiki?id=4_3_default_sensor_node_firmware.md)
* [STEVAL-MKBOXPRO product page](https://www.st.com/en/evaluation-tools/steval-mkboxpro.html)
