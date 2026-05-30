# Lab 2 — Train Your Own MLC Model with ST AIoT Craft <br> /IOTCONNECT Bridge App + ST SensorTile.box PRO

This is the second of two labs. In **[Lab 1](./st_aiotcraft_lab1_deploy.md)** you stood up an /IOTCONNECT account, paired the SensorTile.box PRO over BLE, pushed a starter MLC model, built a dashboard, and watched the full device → BLE → Bridge App → /IOTCONNECT → dashboard loop come alive. **Lab 2 picks up from there**: capture your own labeled sensor data, let ST AIoT Craft train a custom MLC model on it, and OTA the new model back to the same device — closing the **capture → train → deploy → inference → retrain** loop.

<img src="images/sensortilebox_pro.png" alt="SensorTile.box PRO" width="200"/>

> **NOTE**
> This guide tracks the **AWS POC** instance of /IOTCONNECT (the environment used for the ST AIoT Craft preview). Production release is planned for mid-summer.

## Prerequisites

You should have completed **[Lab 1 — Deploy a Starter MLC Model](./st_aiotcraft_lab1_deploy.md)**. Specifically:

- /IOTCONNECT POC account with the **STAIOT** association created (Lab 1 Step 4)
- /IOTCONNECT Bridge App installed and logged in (Lab 1 Steps 5–6)
- SensorTile.box PRO paired and registered in /IOTCONNECT (Lab 1 Steps 7–9)
- A starter model pushed to the device and live inference confirmed end-to-end on the phone and on the dashboard (Lab 1 Steps 10–13)
- The dashboard you imported in Lab 1 Step 12 — you'll come back to it once your custom model is running

You will also need a **USB-C or Lightning microSD card reader** that plugs into your phone — uploading captured sessions to /IOTCONNECT requires popping the microSD out of the box and reading it from the phone (Step 2 below).

## 1. Capture Labeled Sensor Data for Training

This is the half of the loop that feeds **ST AIoT Craft**. Logging mode swaps the device out of inference and streams raw sensor samples into a session that gets zipped and uploaded to /IOTCONNECT.

**Switch the box into data-logging mode**

In the Bridge App, get to the **AI Model List** screen, make sure **Smart Asset Tracking** is selected, then tap **Data Logging** at the bottom (highlighted below). Two ways to get there depending on where you're starting from:

* **Coming straight from Lab 1.** The box is still in inference mode. Tap the back arrow on the inference view to return to the **AI Model List** — **Smart Asset Tracking** is still selected and the **Data Logging** button is active.
* **Starting Lab 2 fresh (re-opened the app, came back later).** The **AI Model List** may load with **Run Inference** and **Data Logging** greyed out — the app waits for an active model deployment before either button lights up. Re-push **Smart Asset Tracking** from /IOTCONNECT (see [Lab 1 Step 10](./st_aiotcraft_lab1_deploy.md#10-push-the-smart-asset-tracking-model)). Once the Bridge App receives the OTA over BLE (a few seconds), the buttons activate.

<img src="images/st_aiotcraft/ai_model_list_datalogging_annotated.png" alt="AI Model List — tap Data Logging" width="240"/>

**Pick the sensor**

On the Select Sensor screen:

1. Tap **① Accelerometer**. The Start button activates as soon as a sensor is selected.
2. Tap **② Start**.

<img src="images/st_aiotcraft/39_select_sensor_annotated.png" alt="Select Sensor — tap Accelerometer then Start" width="240"/>

**Record one labeled segment per class — ~10 seconds each**

The next screen shows all four classes as tags at the top — *Stationary Upright*, *Stationary Not Upright*, *Motion*, *Shaken* — over a live chart of the accelerometer stream. Logging is already active when you land here. Work through all four tags **in sequence**, ~10 seconds per tag, then stop:

<img src="images/st_aiotcraft/40_select_tags_annotated.png" alt="Tags screen — work through ①…④ then ⑤ Stop" width="240"/>

1. Tap **① Stationary Upright** and stand the box upright on a flat surface. Hold ~10 seconds.
2. Tap **② Stationary Not Upright** and lay the box on its side (or upside down). Hold ~10 seconds.
3. Tap **③ Motion** and move the box around — slide it, rotate it, walk with it. ~10 seconds.
4. Tap **④ Shaken** and shake the box briskly. ~10 seconds.
5. After all four tags are done (each shows a green checkmark and the chart fills with data, as below), tap **⑤ Stop** to end the session.

<img src="images/st_aiotcraft/41_logging_active.png" alt="All four tags captured — ready to Stop" width="240"/>

> **Labeling — 4 Simple Rules**
> 1. **Pick at least 2 labels.** Single-label training fails with "job failed" — this is the #1 cause. (Doing all four as above is well above the minimum.)
> 2. **At least 5 seconds per label.** Less than 5s → no association → upload may parse but training will reject it. (~10s per tag leaves a safe margin.)
> 3. **Multiple labels can overlap.** If you press *Shaken* and then press *Motion* without releasing *Shaken*, both get entries in `acquisition_info.json`. The first label is **not** auto-released — overlap is intentional.
> 4. **Re-pressing a label that already logged is ignored.** If you select *Shaken*, log it, deselect, then re-select *Shaken*, the second selection adds nothing — the cloud already has *Shaken* data for this session.

Stopping the session leaves the labeled data on the SensorTile.box PRO's microSD card — it does **not** auto-upload. **Step 2** walks through pushing it to /IOTCONNECT so AIoT Craft can train on it.

## 2. Push Samples to /IOTCONNECT

To kick off training, the session files have to move from the device's microSD card up to /IOTCONNECT. The Bridge App's **SD Card** screen walks you through it.

1. **Stop training.** Confirm you're back at the *Accelerometer* screen with logging stopped and your tags still selected.

   <img src="images/st_aiotcraft/41_logging_active.png" alt="Stop logging" width="240"/>

2. **Find the log folder.** Open the **SD Card** section in the Bridge App. You'll see a "Follow these steps" help screen explaining the dongle workflow.

   <img src="images/st_aiotcraft/44_sd_help.png" alt="SD Card help" width="240"/>

3. **Move the microSD card from the box to your phone.** This part trips people up — the card has to physically come out of the SensorTile.box PRO, go into a microSD-to-USB dongle, and that dongle plugs into your phone. Your phone does **not** read the card wirelessly.

   <img src="images/st_aiotcraft/sd_card_workflow.png" alt="Move the microSD from the SensorTile.box PRO to your phone — 4-step physical workflow" width="900"/>

   1. **Power off the box** and open the enclosure. The microSD slot is on the side **opposite the gold capacitive touch pads** — the gold patterns visible on one side of the board are touch sensors, *not* the SD slot.
   2. **Push the microSD card to release it** and pull it out. Set the empty box aside.
   3. **Insert the microSD into a dongle** — a USB-C dongle for Android or newer iPhones, a Lightning dongle for older iPhones.
   4. **Plug the dongle into your phone.**

   Back in the Bridge App, tap **Connect and Select Root**, grant file access, and browse to the `STM32` folder — each logging session is a date-named folder (e.g. `20260429_21_30_06`).

   <img src="images/st_aiotcraft/42_sd_sessions.png" alt="SD card folder list" width="240"/>

4. **Select / push the files.** Open the most recent session folder, tick all three files (`acquisition_info.json`, `device_config.json`, and `lsm6dsv16x_acc.dat` / `_gyro.dat`), and push.

   <img src="images/st_aiotcraft/45_select_files.png" alt="Select files" width="240"/>

5. **Upload Success.** When the bundle has been zipped and accepted by /IOTCONNECT, the app shows an **Upload Success** confirmation. The session is now queued for AIoT Craft to consume.

   <img src="images/st_aiotcraft/46_upload_success.png" alt="Upload Success" width="240"/>

> **NOTE**
> The upload is the trigger for AIoT Craft to start training. If a session never makes it into /IOTCONNECT, no `.ucf` model will come back — make sure each labeled session ends with the **Upload Success** popup.

## 3. Inspect a Logged Session

Each session is a folder on the device's SD card (and a zip in /IOTCONNECT) containing three artifacts:

| | |
|---|---|
| <img src="images/st_aiotcraft/42_sd_sessions.png" alt="SD card sessions" width="240"/> | <img src="images/st_aiotcraft/43_session_files.png" alt="Session files" width="240"/> |

* **`acquisition_info.json`** — every label you pressed, with start/stop timestamps. The cloud uses this to slice the raw `.dat` files into labeled chunks.
* **`device_config.json`** — sensor configuration (ODR, full-scale, enabled axes) at recording time. The cloud needs this to interpret the binary correctly.
* **`lsm6dsv16x_acc.dat` / `lsm6dsv16x_gyro.dat`** — raw binary samples, parsed and chunked into CSV by AIoT Craft.

To find the upload in /IOTCONNECT, open your device's Device Info page and click the **Telemetry Files** tab. Each row is one logging session with timestamp, size, and a download link.

> **NOTE**
> If a freshly stopped session doesn't appear immediately, give it a minute — the upload happens after the session closes, not during recording.

## 4. The MLC Retraining Loop

Once one or more labeled sessions are sitting in /IOTCONNECT, the connector you set up in **Lab 1 Step 4** forwards the binary HSD data to ST AIoT Craft, which runs AutoML / AFS to produce a `.ucf` MLC model. AIoT Craft pushes that model **back** into your account's AI Model Library, and a single **Push Model** click delivers it to the device:

```
[Device]            [BLE]      [Bridge App]   [/IOTCONNECT]   [Connector]   [ST AIoT Craft]
   │                  │             │              │              │              │
   │ MLC inference ──►│ ───────────►│ Telemetry    │              │              │
   │                  │             │ ───────────► │ Stored as    │              │
   │                  │             │              │ template     │              │
   │ Switch→logging ◄─┤ ◄───────────┤ Cmd from     │              │              │
   │ datalog2 firmware│             │ cloud        │              │              │
   │                  │             │              │              │              │
   │ Raw .dat + JSON ►│ ───────────►│ Zip + upload │              │              │
   │                  │             │ ───────────► │ Forward via  │              │
   │                  │             │              │ connector ──►│ Blob ingest  │
   │                  │             │              │              │ (binary-hsd) │
   │                  │             │              │              │ ───────────► │ Chunks (CSV)
   │                  │             │              │              │              │ AutoML / AFS
   │                  │             │              │              │              │ trains MLC
   │                  │             │              │              │ ◄────────────┤ .ucf model
   │                  │             │              │ Model ◄──────┤              │
   │                  │             │              │ registered   │              │
   │                  │             │              │ in library   │              │
   │ load_model PnPL ◄┤ ◄───────────┤ Push model   │              │              │
   │ command          │             │ (OTA)        │              │              │
   │                  │             │              │              │              │
   │ MLC reprogrammed,│             │              │              │              │
   │ inference resume►│ ───────────►│ ───────────► │ Live again   │              │
```

To verify a freshly trained model:

1. /IOTCONNECT → **AI Models → Push Model** → select your new model → **Push**.
2. In the Bridge App, return to **AI Model List** (the new model appears) and tap **Run Inference** to switch the box back into inference mode.
3. Open the dashboard you built in **Lab 1 Step 12** — your model is now running on real hardware, and the classifications and raw telemetry flow into the same widgets you exercised at the end of Lab 1.

> **Capture → Upload → Train → Deploy → Inference → (capture more) → Retrain**

## 5. Find Your Trained Model in /IOTCONNECT

When AIoT Craft finishes training (typically ~30 seconds after the upload is accepted), a new entry appears in your AI Model library — distinct from the read-only **Model Library** used in **Lab 1 Step 10**, this is the **My Model** list of models trained on your own data.

1. From the left menu open **AI Models → AI Model**.

   <img src="images/st_aiotcraft/47_my_model_menu.png" alt="AI Models → AI Model" width="700"/>

2. Your trained model shows up in the **My Model** tab as soon as training completes — typically within ~30 seconds. Status reads **Completed** when it's ready to push.

   <img src="images/st_aiotcraft/49_my_model.png" alt="My Model list" width="700"/>

3. Click the version number to see every training run AIoT Craft has produced for this model — useful when you've captured multiple datasets and want to compare or roll back.

   <img src="images/st_aiotcraft/48_version_list.png" alt="Version List" width="700"/>

> **NOTE**
> If the model doesn't appear within a couple of minutes, check that the STAIOT association from **Lab 1 Step 4** is still active and that the upload completed (Step 2 above ended in **Upload Success**). The single-label-fails / under-5-second rules from **Step 1** also surface here as a *job failed* status.

From this point, deploying the model is the same one-click **Push Model** flow you used in **Lab 1 Step 10** — except the model is now the one trained on your own data.

## 6. The 5 Stages — End-to-End

Putting both labs on one page:

| Stage | Time | What you did | Where |
|---|---|---|---|
| **1. Connect** | ~10 min | Account · STAIOT association · BLE pair · template/device verified | Lab 1, Steps 1–9 |
| **2. Deploy a model** | ~10 min | OTA push a starter model · live inference on device | Lab 1, Steps 10–11 |
| **3. Visualize & verify** | ~10 min | Import dashboard · confirm classifications + raw telemetry end-to-end | Lab 1, Steps 12–13 |
| **4. Capture data** | ~15 min | Switch into logging mode · record labeled sessions · push samples via dongle | Lab 2, Steps 1–3 |
| **5. Train & redeploy** | ~15 min | AIoT Craft trains a model · find it under My Model · OTA back · see your model run | Lab 2, Steps 4–5 |

## Optional — Build Your Own Custom Experiences

/IOTCONNECT exposes everything used in this guide via REST and AWS-native connectors:

* REST API docs: <https://docs.iotconnect.io/iotconnect/rest-api/>
* Swagger (POC): <https://awspocmaster.iotconnect.io/api/v2.1/swagger-json>
* AWS connectors: S3, DynamoDB, SNS, SageMaker, Kinesis, Lambda, Greengrass, EventBridge, Grafana

Webhooks for inbound events, OAuth 2.0 across the API, and CI/CD-friendly OTA triggers mean every step in this guide can be scripted.

## See Also

* [Lab 1 — Deploy a Starter MLC Model](./st_aiotcraft_lab1_deploy.md) — the connect + deploy + dashboard half of this loop
* [Main Mobile App Guide](./mobile_app_guide.md) — the production AWS flow with `BLESensorsPnPL.bin` / `STSW-MKBOXPRO_1_1_1.bin` firmwares
* [SensorTile.box PRO Getting Started Guide](https://www.st.com/resource/en/user_manual/um3133-getting-started-with-sensortilebox-pro-multisensors-and-wireless-connectivity-development-kit-for-any-intelligent-iot-node-stmicroelectronics.pdf) — DFU mode, hardware reference
* [/IOTCONNECT Product Updates](https://docs.iotconnect.io/iotconnect/platform/product-updates/) — AIoT Craft GA timeline
