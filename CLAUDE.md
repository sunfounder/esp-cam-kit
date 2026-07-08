# CLAUDE.md - ESP32 Camera Pro Kit Documentation

## Project Overview

This repository contains RST documentation for the ESP32 Camera Pro Kit. The documentation is built with Sphinx and includes project tutorials under `docs/source/projects/`.

This is the **docs-de** (German) branch. All changes synced from `docs-en`.

## Replacing Arduino Cloud iframe Embeds with Inline Code

### Background

The project tutorial `.rst` files previously used embedded Arduino Cloud iframes to display code. These have been replaced with inline `.. code-block:: arduino` directives containing the actual code.

### Source of Code

The actual Arduino code (`.ino` files) lives in the copy directory:
`f:\Basic Kits\Arduino\ESP32 Camera Pro Kit\esp-cam-kit - Copy\related_projects\`

### Category A: Single .ino File Projects (Inline Code)

| RST File | .ino Source |
|---|---|
| `ar_blinking_led.rst` | `2.1_hello_led/2.1_hello_led.ino` |
| `ar_fading.rst` | `2.2_fading_led/2.2_fading_led.ino` |
| `ar_button.rst` | `2.3_digital_input/2.3_digital_input.ino` |
| `ar_pot.rst` | `2.4_analog_input/2.4_analog_input.ino` |
| `ar_lcd.rst` | `2.5_lcd_interface/2.5_lcd_interface.ino` |
| `ar_motor.rst` | `2.6_drive_a_motor/2.6_drive_a_motor.ino` |
| `ar_servo.rst` | `2.7_driver_a_servo/2.7_driver_a_servo.ino` |
| `ar_bluetooth.rst` | `2.8_use_the_bluetooth_function/2.8_use_the_bluetooth_function.ino` |
| `ar_bluetooth_audio_player.rst` | `2.9_bluetooth_player/2.9_bluetooth_player.ino` |
| `ar_sd_read_write.rst` | `2.10_sd_card_write_and_read/2.10_sd_card_write_and_read.ino` |
| `ar_mp3_player_sd.rst` | `2.11_mp3_player_with_sd_card/2.11_mp3_player_with_sd_card.ino` |
| `ar_take_photo_sd.rst` | `2.12_take_photo_sd/2.12_take_photo_sd.ino` |
| `ar_iot_blynk.rst` | `2.15_blynk_based_intrusion_notification_system/2.15_blynk_based_intrusion_notification_system.ino` |

#### Replacement Pattern (Single File)

**Old** (remove):
```rst
    .. note::
        * :ref:`unknown_com_port`

    .. raw:: html
        <iframe src=...></iframe>
```

**New** (insert):
```rst
   .. code-block:: arduino

        <code from .ino file, 8 spaces indent>
```

#### Key Rules
1. Remove `.. note::` with `:ref:`unknown_com_port`` AND `.. raw:: html` iframe from code section
2. Move `:ref:`unknown_com_port`` to after code block (inside "connect ESP32" step)
3. Add `.. code-block:: arduino` with 8-space-indented .ino code
4. Preserve `|link_download_this_code|` line

#### ⚠️ `ar_motor.rst` — Special Case
Has **TWO** iframes:
- **Main code section**: Replace with inline code (normal Category A)
- **"Learn More" section**: Just remove the iframe, keep the `.. note::` (source `4.1_motor_pwm.ino` not in copy directory)

---

### Category B: Download-Only Projects

| RST File | Project Dir | Reason |
|---|---|---|
| `ar_iot_camera_web.rst` | `2.13_camera_web_server/` | Multi-file |
| `ar_iot_html_cam_led.rst` | `2.14_custom_video_streaming_web_server/` | Code ~400 lines |

#### Replacement Pattern
**Old**: `|link_download_this_code| or copy this code...` + note + iframe
**New**: `|link_download_this_code|. After downloading, extract the zip file and open the \`\`<project_dir>.ino\`\` file in the Arduino IDE.` + note only (no iframe, no code-block)

---

## 18650 Battery References Removal

### `component_esp32_extension.rst`
- Remove "18650" from text: "3.7V 18650 battery" → "3.7V battery"
- Comment out entire **"Battery Power and Charging"** section with `.. ` prefix

### `index.rst`
- Replace `img/battery_charge.png` → `img/esp32_camera.png`
- Remove `component_battery` from toctree

### Battery Steps in Project Files
- `ar_iot_html_cam_led.rst`: Comment out the last step (insert battery + `plugin_battery.png`)

---

## Image Alignment Cleanup (`ar_iot_blynk.rst`)

- Remove ALL `:align: center` from image directives (~25+ images)
- Remove `:width: XX%` percentage widths, use absolute pixels
- **Section 2.1 images** (signup flow): NO `:width:` at all
- **Section 2.2-2.7 images** (template config): `:width: 600`
- **Circuit image** (`iot_9_blynk_bb.png`): `:width: 400` (was `600` on ja branch — check!)
- **Code block**: Remove license header from .ino code (keep code starting from `#define BLYNK_PRINT Serial`)

---

## Image Files to Sync from `docs-en`

These binary files need to be copied from `docs-en` branch:
- `docs/source/img/esp32_camera.png` — new image for index.rst
- `docs/source/projects/img/4.1_motor_l293d_bb.png` — updated motor wiring diagram

```bash
git restore --source=docs-en -- docs/source/img/esp32_camera.png
git restore --source=docs-en -- docs/source/projects/img/4.1_motor_l293d_bb.png
```

---

## Config Files to Update

- **`conf.py`**: Project name → "SunFounder ESP32 Camera Kit"; comment out `autosectionlabel`
- **`requirements.txt`**: `sphinx_rtd_theme==3.0.2` (already `sphinx==7.3.7`)
- **`.readthedocs.yaml`**: Add `submodules:` block; change `formats: all` → `formats: [epub, pdf]`
- **`.gitignore`**: Replace with expanded version (`.vscode`, `build*`, `build/`, etc.)

---

## ⚠️ Lessons Learned from docs-ja Sync

These were MISSED in the initial docs-ja sync and fixed later:

1. **`4.1_motor_l293d_bb.png`** — Image file was not copied from docs-en initially
2. **`ar_iot_blynk.rst` license header** — Code block had full Blynk license header; should be stripped to match docs-en
3. **`ar_iot_blynk.rst` circuit image width** — Was `600`, should be `400`
4. **`ar_iot_blynk.rst` section 2.1 images** — `09_blynk_access.png` and `09_blynk_tour.png` had `:width: 600` that shouldn't be there
