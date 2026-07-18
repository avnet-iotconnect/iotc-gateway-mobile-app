# Engineering Note — SensorTile.box PRO Hardware Revisions, DATALOG2 Firmware & BlueST SDK Compatibility

This note explains **why the AIoT Craft labs standardize on FP-SNS-DATALOG2_Datalog2 v3.1.0**, how the SensorTile.box PRO identifies itself over Bluetooth, and which hardware / firmware / mobile-app combinations work today. It applies to the [ST AIoT Craft guide](./st_aiotcraft_guide.md), [Lab 1](./st_aiotcraft_lab1_deploy.md), and [Lab 2](./st_aiotcraft_lab2_train.md), and is **cloud-agnostic** — the same rules hold whether the Bridge App is signed into the AWS POC, production AWS, or an Azure /IOTCONNECT environment (see [Section 6](#6-cloud-environments-aws-poc-production-aws-azure)).

> **Last verified: July 2026.** The published SDK and catalog state described below changes as ST ships releases — re-verify the version-specific claims before relying on them later.

## TL;DR — the operating rules

1. **Every box runs FP-SNS-DATALOG2_Datalog2 v3.1.0**, regardless of hardware revision (A, B, or C). It is the only DATALOG2 release recognized by the Bridge App on **both** iOS and Android today.
2. **Do not let the ST AIoT Craft mobile app "update" a box.** It force-installs v3.2.0, which the Bridge App on iOS cannot recognize. If a box gets updated, reflash v3.1.0 ([Section 7](#7-flashing-reference)).
3. **Box names are exactly 7 characters, letters and numbers only.** The firmware stores names in a fixed 7-byte slot with no validation — shorter names get garbage bytes appended (persisted in flash, surviving reflash), longer names corrupt memory.
4. **Never delete the box's device record in the /IOTCONNECT console.** The app only registers a device on first pairing and holds the auto-generated certificates — after a console-side delete, the app loops "connecting / disconnected" (~20 s cycle) forever. Recovery: uninstall and reinstall the Bridge App, then re-pair.

## 1. Hardware revisions

ST has shipped three revisions of the SensorTile.box PRO ([STEVAL-MKBOXPRO](https://www.st.com/en/evaluation-tools/steval-mkboxpro.html)). The sticker on the enclosure shows the order code:

| Revision | Order code | Hardware change | Sensor set |
|---|---|---|---|
| Rev A | STEVAL-MKBOXPRO**A** | Baseline (NFC: ST25DV04K, BLE: BlueNRG-LP) | LSM6DSV16X, LIS2DU12, LIS2MDL, LPS22DF, STTS22H, MP23DB01HP |
| Rev B | STEVAL-MKBOXPRO**B** | NFC IC changed to ST25DV04**KC** | identical |
| Rev C | STEVAL-MKBOXPRO**C** | BLE chip changed to STM32WB07 | identical |

The sensors — and therefore everything the labs exercise (MLC models, data logging, inference) — are **identical across revisions**. The revisions matter only because of how the firmware announces itself, below.

## 2. How apps recognize a box — BlueST identity and the catalog

Per the [BlueST Protocol v2](https://staiotcraft.st.com/index.html#/wiki?id=4_5_bluest_protocol.md), the box advertises a **(board ID, firmware ID)** byte pair over BLE. Client apps look that pair up in ST's firmware catalog ([STMicroelectronics/appconfig](https://github.com/STMicroelectronics/appconfig), `bluestsdkv2/catalog.json`) to learn what the device is and which features it exposes. Two consequences:

- **A pair the app's catalog doesn't contain = an unrecognized device.** The app falls back to raw BLE feature names (`MachineLearningCoreFeature`, `HSDFeature`, `RawPnPLControlledFeature`) or worse (see [Section 5](#5-failure-signatures)).
- **Apps do not read the live catalog.** Each app build pins a *frozen snapshot branch* (`blesensor_<version>`) — the "**catalog pin**". A stale pin silently blinds the app to newer firmware.

The DATALOG2 firmware detects the hardware revision at boot (via the NFC IC) and selects its advertised identity accordingly. The mapping **changed at v3.2.0** — the separate Rev B identity was retired and Rev B boxes began reporting as Rev C:

| Hardware | Firmware | Advertises (board ID, fw ID) |
|---|---|---|
| Rev A | v3.1.0 | `0x0D` / `0x1D` |
| Rev B | v3.1.0 | `0x11` / `0x18` |
| Rev A | v3.2.0 / v3.3.0 | `0x0D` / `0x26` / `0x28` |
| Rev B **and** C | v3.2.0 / v3.3.0 | `0x13` / `0x08` / `0x0A` |

(Board IDs are official: `0x0D` = MKBOXPRO, `0x11` = MKBOXPROB, `0x13` = MKBOXPROC — see the Device Id table in the BlueST protocol wiki. Firmware detection source: [fp-sns-datalog2](https://github.com/STMicroelectronics/fp-sns-datalog2), `Projects/STM32U585AI-SensorTile.boxPro/Applications/DATALOG2/Core/Src/App.c` per tag.)

## 3. DATALOG2 firmware versions

| Version | Released | Box-PRO-relevant changes |
|---|---|---|
| **v3.1.0** | Jul 2025 | **The labs' standard.** Baseline for AIoT Craft. |
| v3.2.0 | Dec 2025 | DFU-entry fix; **identity change** (Rev B reports as Rev C). New sensors target STWIN.box / Nucleo shields, not the box PRO. |
| v3.3.0 | Jun 2026 | SD-close timeout fix; IIS3DWB10IS **add-on** support. Nothing for the onboard sensor set. |

**v3.2.0 and v3.3.0 add no capability the labs use.** The onboard sensors, MLC flow, BLE streaming, and SD logging are unchanged. Moving past v3.1.0 buys nothing and breaks iOS recognition (next section) — hence the standardization.

## 4. BlueST SDK state (as of July 2026)

The Bridge App builds on ST's BlueST SDKs. Their published states diverge sharply by platform:

| | Android — [BlueSTSDK_Android](https://github.com/STMicroelectronics/BlueSTSDK_Android) | iOS — [BlueSTSDK_iOS](https://github.com/STMicroelectronics/BlueSTSDK_iOS) |
|---|---|---|
| Release cadence | ~every 2 months (V1.2.8 → V1.2.18, Dec 2024–Apr 2026) | 3 releases since 2023; latest **v1.1.1 (Nov 2025)** |
| Catalog pin | `blesensor_5.3.0` (has all v3.2.0+ entries) | defaults to `blesensor_5.2.0` (**no** v3.2.0+ entries) |
| Rev C board type (`0x13`) | yes | only from v1.1.1 |
| AIoT Craft firmware allowlist | none found | hardcoded `["3.1.0"]` (`Firmware.swift`) |

The resulting compatibility matrix for the Bridge App:

| Firmware on box | Android | iOS |
|---|---|---|
| v3.1.0 (any revision) | ✅ | ✅ |
| v3.2.0 / v3.3.0 (any revision) | ✅ | ❌ |

Two further facts worth knowing:

- **ST's own AIoT Craft mobile app installs v3.2.0** — it is built from a newer internal SDK state that has not been published. Third-party apps can only build from the public repos, which predate every currently shipping DATALOG2 release. (This is also why the app-side fix is ST's to unblock: publish the updated SDK, or share the app's SDK state.)
- The current iOS app source ST publishes ([STBLESensor_iOS](https://github.com/STMicroelectronics/STBLESensor_iOS)) pins catalog `5.3.0` via `CFBundleCatalogVersionString` — the reference implementation for any iOS app updating its pin.

## 5. Failure signatures

Symptoms observed during lab preparation, with root cause and fix:

| Symptom | Cause | Fix |
|---|---|---|
| Selecting the device shows `MachineLearningCoreFeature`, `HSDFeature`, `RawPnPLControlledFeature`; no AI Model List | Unknown **board ID** — box is on v3.2.0+ and the app's SDK/catalog predates its identity | Reflash v3.1.0 |
| Box connects and disconnects in a BLE loop on pairing (iOS) | Known board, unknown **firmware ID** — app engages its flow, can't resolve the firmware, retries forever | Reflash v3.1.0 |
| App shows "connecting to /IOTCONNECT… disconnected" every ~20 s, never connects | Device record deleted in the console; app retries against a registration that no longer exists (it holds the certificates, so console-side re-creation can't help) | Uninstall + reinstall the Bridge App, sign in, re-pair |
| Box advertises a corrupted name / cloud rejects the device ID after a rename | Firmware stores names as a fixed 7-byte copy with no length validation | Re-rename with **exactly 7** alphanumeric characters |

The box's name is stored in a dedicated flash page (`0x080FE000`) and **survives reflashing** — reflashing firmware neither fixes nor breaks a name.

## 6. Cloud environments: AWS POC, production AWS, Azure

Everything in Sections 1–5 lives on the **BLE side of the bridge** and is **identical across cloud environments** — the box neither knows nor cares which /IOTCONNECT instance the phone is signed into. What changes per environment is the cloud half:

| Environment | Console | Used by | AIoT Craft flow |
|---|---|---|---|
| AWS POC | `awspoc.iotconnect.io` | [AIoT Craft guide](./st_aiotcraft_guide.md) / [Lab 1](./st_aiotcraft_lab1_deploy.md) / [Lab 2](./st_aiotcraft_lab2_train.md) | ✅ (preview environment) |
| Production AWS | `console.iotconnect.io` | [Main mobile app guide](./mobile_app_guide.md) | Planned mid-summer |
| Azure | per-tenant /IOTCONNECT Azure instance | selectable in the Bridge App login | Not available (AIoT Craft preview is AWS-POC-only) |

Environment-portability rules:

- **Device records, templates (`AvnetSTaws`), models, and dashboards are per-environment and per-account.** Nothing migrates when you switch the app's login environment — pairing a box while signed into a different environment creates a fresh registration there.
- The firmware version rule (v3.1.0) and naming rule (7 chars) apply **unchanged** in every environment, because recognition happens before the cloud is ever involved.
- The [failure signatures](#5-failure-signatures) table applies unchanged in every environment for the same reason — with one addition: the "deleted device record" loop is per-environment, so a box can be healthy in one environment and stranded in another.

## 7. Flashing reference

1. Enter DFU mode: hold the BOOT button while plugging in USB-C ([UM3133 §2.2](https://www.st.com/resource/en/user_manual/um3133-getting-started-with-sensortilebox-pro-multisensors-and-wireless-connectivity-development-kit-for-any-intelligent-iot-node-stmicroelectronics.pdf)). The board enumerates as "STM32 BOOTLOADER".
2. Flash the stock v3.1.0 binary — shipped by ST inside the [fp-sns-datalog2 v3.1.0 tag](https://github.com/STMicroelectronics/fp-sns-datalog2/tree/v3.1.0/Projects/STM32U585AI-SensorTile.boxPro/Applications/DATALOG2/Binary) (`DATALOG2_Release.bin`) — at address `0x08000000` with [STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html):

   ```
   STM32_Programmer_CLI -c port=usb1 -w DATALOG2_Release.bin 0x08000000 -v -rst
   ```

3. The box's 7-character name survives the reflash (it lives outside the firmware image). Verify the box reappears in the Bridge App under its printed identifier.

## Sources

* [fp-sns-datalog2](https://github.com/STMicroelectronics/fp-sns-datalog2) — firmware source, tags v3.1.0–v3.3.0 (identity logic: `App.c`, ID values: `sysconfig.h`)
* [appconfig](https://github.com/STMicroelectronics/appconfig) — the BlueST firmware catalog; snapshot branches `blesensor_*`
* [BlueSTSDK_Android](https://github.com/STMicroelectronics/BlueSTSDK_Android) / [BlueSTSDK_iOS](https://github.com/STMicroelectronics/BlueSTSDK_iOS) / [STBLESensor_iOS](https://github.com/STMicroelectronics/STBLESensor_iOS) — SDK sources, catalog pins, allowlist
* [ST AIoT Craft wiki — BlueST Protocol](https://staiotcraft.st.com/index.html#/wiki?id=4_5_bluest_protocol.md) and [Default Sensor Node Firmware](https://staiotcraft.st.com/index.html#/wiki?id=4_3_default_sensor_node_firmware.md)
* [STEVAL-MKBOXPRO product page](https://www.st.com/en/evaluation-tools/steval-mkboxpro.html)
