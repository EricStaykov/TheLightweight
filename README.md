# The Lightweight

The Lightweight is a running/walking/cycling light with four RGB (red, green, blue) lights arranged in a square. It can display up to ten sequences that are fully customisable using the software.

---

## Table of Contents

- [Requirements](#requirements)
- [Device Operation)(#device-operation)
- [The Lightweight - Pattern Programmer](#software-intro)
  - [Windows](#windows)
  - [macOS](#macos)
- [Connecting Your Device](#connecting-your-device)
- [Interface Overview](#interface-overview)
- [Building a Pattern](#building-a-pattern)
- [Previewing a Pattern](#previewing-a-pattern)
- [Working with the Device](#working-with-the-device)
  - [Set Active Sequence](#set-active-sequence)
  - [Live Colour Preview](#live-colour-preview)
  - [Upload / Download a Single Sequence](#upload--download-a-single-sequence)
  - [Download All / Upload All Sequences](#download-all--upload-all-sequences)
  - [Reset Patterns to Default](#reset-patterns-to-default)
- [The Sequence File Format](#the-sequence-file-format)
- [Menu Bar](#menu-bar)
- [Troubleshooting](#troubleshooting)
- [FAQ](#faq)

---

## Device Operation

Once configured, The Lightweight device can be operated independently of the computer using its built-in touch sensor.

* **Power Toggle:** Tap the sensor using your assigned power configuration to turn the device on or off, which defaults to 2 taps.

* **Mode/Sequence Cycling:** Tap the sensor using your assigned mode/sequence configuration to cycle through your enabled sequences, which defaults to 1 tap.

---

## The Lightweight - Pattern Programmer

This is a desktop application for designing, previewing and uploading custom LED lighting patterns to The Lightweight device over USB. Build animations visually by dragging colour-and-timing "step blocks" into a sequence, preview them on screen, then push them straight to the device's memory.

**Requirements:**

- Windows 10/11 (64-bit) or macOS 15+
- The Lightweight device connected via a USB-C cable
- No other software needs to be installed - the `.exe` / `.app` bundles everything required to run

> Note: On some systems the device driver stack (or macOS security settings) may need a one-time approval the first time you connect. See [Troubleshooting](#troubleshooting).

---

## Installation

### Windows

1. Download the latest `TheLightweight.exe` from the [Windows App](https://github.com/EricStaykov/TheLightweight/) page.
2. Double-click the `.exe` to launch it - no installer step is required.
3. If Windows SmartScreen shows a warning, click More info → Run anyway.

### macOS

1. Download the latest `TheLightweight-macOS.zip` from the [macOS App](https://github.com/EricStaykov/TheLightweight/) page.
2. Unzip it and drag `TheLightweight.app` into your Applications folder.
3. Because the app didn't come from the Mac App Store, the first launch requires one extra step:
   - Right-click (or Control-click) the app icon and choose Open, then confirm Open in the dialog that appears.
   - If macOS still blocks it, go to System Settings → Privacy & Security and click Open Anyway next to the warning about `TheLightweight.app`.

---

## Connecting Your Device

1. Plug The Lightweight device into your computer with a USB-C cable.
2. Launch the app.
3. In the Device Connection panel (right-hand side), click Connect.
4. A green dot and the text Connected confirm a successful connection. A red dot means the app could not open the device - see [Troubleshooting](#troubleshooting).
5. Click Disconnect at any time to release the device (recommended before unplugging it).

---

## Interface Overview

The window is split into two halves:

| Side | Panel | Purpose |
|---|---|---|
| Left | Pattern Builder | Design a sequence of colour/timing steps and preview it on screen |
| Right | Device Panel | Connect to the hardware and send/receive patterns, live colours and sequence changes |

A menu bar across the top gives you theme switching, help, update checks and app info.

---

## Building a Pattern

The Pattern Builder is where you design an animation before sending it to the device.

1. Drag a block in. In the palette box labelled *"Drag a block into the sequence below,"* drag the New Step Block tile down into the Pattern Sequence list. Each block you drop becomes one step in the animation.
2. Edit a step. Click Edit on a step block to open its editor:
   - LED Colours - four colour swatches, one per physical LED position (Top-Left, Top-Right, Bottom-Right, Bottom-Left). Click a swatch to open the colour picker.
   - Timing - set the step's Duration (in milliseconds) and its Type:
     - Solid - the four LED colours are held steady for the full duration.
     - Transition - the LEDs fade from the previous step's colours into this step's colours over the duration.
3. Reorder steps. Drag a step block up or down within the sequence list to change playback order.
4. Duplicate a step. Click Duplicate on any step block to insert a copy of it right after itself - handy for making small variations quickly.
5. Delete a step. Click Delete to remove a step block from the sequence.
6. Clear everything. Click Clear All Steps to empty the whole sequence (with a confirmation, since this can't be undone).

Each step block shows a small live preview of its four colours and its duration so you can scan the whole sequence at a glance.

---

## Previewing a Pattern

Above the sequence list, the big Preview square shows the same 2×2 LED layout as the physical device:

```
[ Top-Left ]     [ Top-Right ]
[ Bottom-Left ]  [ Bottom-Right ]
```

- Click ▶ Play Preview to play the current sequence on screen exactly as it will run on the device (respecting each step's duration and solid/transition type).
- Click ■ Stop to stop playback at any time.

This lets you fine-tune timing and colours before spending time uploading to hardware.

---

## Working with the Device

All of the controls below live in the Device Panel on the right and require an active connection.

### Set Active Sequence

The device stores up to 10 sequences (1–10). Use the Set Active Sequence box to pick a Sequence# and click Set to make that slot the one currently playing on the device.

### Live Colour Preview

Use the four colour swatches in Live Colour Preview to pick a one-off colour for each LED, then click Live Preview to instantly push that single frame to the physical device - useful for testing colours in real time without building or saving a full sequence.

### Upload / Download a Single Sequence

- ↑ Upload Sequence to The Lightweight - sends the pattern currently in the Pattern Builder to the Sequence# you select, overwriting whatever was stored there.
- ↓ Download Sequence into Builder - pulls whatever is stored in the selected Sequence# on the device back into the Pattern Builder, so you can inspect or edit it.

### Download All / Upload All Sequences

For backing up or restoring every sequence at once:

- ↓ Download All Sequences - reads all 10 sequences off the device and saves them to a `.txt` text file of your choosing. Use this to back up your device's configuration before experimenting.
- ↑ Upload All Sequences - loads a sequence `.txt` file from disk, validates it and (after you confirm) overwrites all 10 slots on the device with its contents. This is destructive and cannot be undone, so the app always asks for confirmation first.

See [The Sequence File Format](#the-sequence-file-format) below if you want to hand-edit these files or generate them programmatically.

### Reset Patterns to Default

Reset Patterns to Default restores the device's built-in factory patterns, erasing any custom sequences you've written. A confirmation dialog protects against accidental clicks - this action cannot be undone.

---

## The Sequence File Format

Sequence `.txt` files are plain text and human-readable, so you can inspect, back up, version-control, or hand-edit them.

```
# THE_LIGHTWEIGHT_SEQUENCES v1
# MAX_MODES=10 MAX_STEPS=64 NUM_LEDS=4

[MODE 1]
STEP type=1 duration=500 colours=255,0,0;0,255,0;0,0,255;255,255,0
STEP type=0 duration=1000 colours=0,0,0;0,0,0;0,0,0;0,0,0

[MODE 2]
```

- Lines starting with `#` are comments and are ignored.
- `[MODE n]` starts a new sequence, where `n` is between 1 and 10. Every sequence is listed once, even if it has no steps.
- Each `STEP` line under a mode defines one animation step:
  - `type` - `0` for Solid, `1` for Transition
  - `duration` - step length in milliseconds (0–65535)
  - `colours` - four semicolon-separated `r,g,b` triples (0–255 each), in the order Top-Left; Top-Right; Bottom-Right; Bottom-Left
- A sequence may have up to 64 steps.

If a file doesn't follow this format exactly, Upload All Sequences will reject it, rather than sending anything to the device.

---

## Menu Bar

| Menu Item | Description |
|---|---|
| Exit | Closes the application |
| Dark Mode / Light Mode | Toggles the app's colour theme |
| Help | Shows a quick-start guide inside the app |
| Check for Updates | Checks GitHub for a newer release and tells you if one is available |
| About | Shows the current app version |

---

## Troubleshooting

"Connect" fails / stays disconnected
- Make sure the device is plugged in via USB-C and powered.
- Try a different USB cable or port - some cables are power-only and don't carry data.
- Make sure no other program (including a previous instance of this app) is already holding the device open.

macOS says the app "can't be opened" or "is damaged"
- Right-click the app and choose Open instead of double-clicking, then confirm in the dialog. See [Installation → macOS](#macos).

Windows SmartScreen blocks the app
- Click More info → Run anyway.

Uploading or downloading sequences randomly fails
- This might be caused by USB noise or a marginal cable/connection dropped or corrupted a byte in transit; the app automatically discards the bad packet. If it happens constantly, try a shorter or better-shielded USB cable.

---

## FAQ

**Can I run this without a physical device?**
Yes - you can build and play back patterns entirely in the Pattern Builder without ever connecting hardware. You only need a connection to write to or read from the device itself.

**How many patterns can the device store?**
10 sequences, each holding up to 64 steps.

**What's the difference between Solid and Transition steps?**
Solid holds a fixed colour for the whole step. Transition fades from the previous step's colours into this step's colours over the step's duration, creating a smooth crossfade.
