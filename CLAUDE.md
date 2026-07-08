# CLAUDE.md - ESP32 Camera Pro Kit Documentation

## Project Overview

This repository contains RST documentation for the ESP32 Camera Pro Kit. The documentation is built with Sphinx and includes project tutorials under `docs/source/projects/`.

This is the **docs-ja** (Japanese) branch. All changes synced from `docs-en`.

## Replacing Arduino Cloud iframe Embeds with Inline Code

### Background

The project tutorial `.rst` files previously used embedded Arduino Cloud iframes to display code. These have been replaced with inline `.. code-block:: arduino` directives containing the actual code.

### Source of Code

The actual Arduino code (`.ino` files) lives in the copy directory:
`f:\Basic Kits\Arduino\ESP32 Camera Pro Kit\esp-cam-kit - Copy\related_projects\`

### Category A: Single .ino File Projects (Inline Code)

These projects contain only a single `.ino` file. The iframe is replaced with inline `.. code-block:: arduino`.

| RST File | .ino Source | Status |
|---|---|---|
| `ar_blinking_led.rst` | `2.1_hello_led/2.1_hello_led.ino` | ✅ Done |
| `ar_fading.rst` | `2.2_fading_led/2.2_fading_led.ino` | ✅ Done |
| `ar_button.rst` | `2.3_digital_input/2.3_digital_input.ino` | ✅ Done |
| `ar_pot.rst` | `2.4_analog_input/2.4_analog_input.ino` | ✅ Done |
| `ar_lcd.rst` | `2.5_lcd_interface/2.5_lcd_interface.ino` | ✅ Done |
| `ar_motor.rst` | `2.6_drive_a_motor/2.6_drive_a_motor.ino` | ✅ Done (Learn More section iframe removed manually) |
| `ar_servo.rst` | `2.7_driver_a_servo/2.7_driver_a_servo.ino` | ✅ Done |
| `ar_bluetooth.rst` | `2.8_use_the_bluetooth_function/2.8_use_the_bluetooth_function.ino` | ✅ Done |
| `ar_bluetooth_audio_player.rst` | `2.9_bluetooth_player/2.9_bluetooth_player.ino` | ✅ Done |
| `ar_sd_read_write.rst` | `2.10_sd_card_write_and_read/2.10_sd_card_write_and_read.ino` | ✅ Done |
| `ar_mp3_player_sd.rst` | `2.11_mp3_player_with_sd_card/2.11_mp3_player_with_sd_card.ino` | ✅ Done |
| `ar_take_photo_sd.rst` | `2.12_take_photo_sd/2.12_take_photo_sd.ino` | ✅ Done |
| `ar_iot_blynk.rst` | `2.15_blynk_based_intrusion_notification_system/2.15_blynk_based_intrusion_notification_system.ino` | ✅ Done |

#### Replacement Pattern (Single File)

**Old pattern** (to be removed):
```rst
    .. note::
        
        * :ref:`unknown_com_port`

    .. raw:: html

        <iframe src=https://create.arduino.cc/editor/sunfounder01/.../preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>
```

**New pattern** (to be inserted):
```rst
   .. code-block:: arduino

        <code from the corresponding .ino file, indented with 8 spaces>
```

#### Key Rules for Single-File Replacement

1. The `.. note::` block containing `:ref:\`unknown_com_port\`` is REMOVED from inside the code section — but the `:ref:\`unknown_com_port\`` reference should be preserved elsewhere in the document (typically moved to after the code block, inside the "connect ESP32" step).
2. The `.. raw:: html` directive with the `<iframe>` is entirely REMOVED.
3. A `.. code-block:: arduino` directive is ADDED in its place, containing the code from the corresponding `.ino` file.
4. The code inside the `.. code-block:: arduino` should be indented with 8 spaces (matching the surrounding RST indentation level).
5. The code should come from the `.ino` file in `esp-cam-kit - Copy/related_projects/<project_dir>/<project_dir>.ino`.
6. Preserve the `|link_download_this_code|` substitution reference line that precedes the code block.

---

### Category B: Download-Only Projects (No Inline Code)

These projects either have multiple files or the code is too long to inline. The user is instructed to download the zip and open the `.ino` file.

| RST File | Project Dir | Reason |
|---|---|---|
| `ar_iot_camera_web.rst` | `2.13_camera_web_server/` | Multi-file (`.ino` + `.cpp` + `.h` + `.csv`) |
| `ar_iot_html_cam_led.rst` | `2.14_custom_video_streaming_web_server/` | Code too long (~400 lines) |

#### Replacement Pattern (Multi-File)

**Old pattern** (to be removed):
```rst
#. |link_download_this_code| or copy this code to the Arduino IDE directly.

    .. note::

        * :ref:`unknown_com_port`

    .. raw:: html

        <iframe src=https://create.arduino.cc/editor/sunfounder01/.../preview?embed style="height:510px;width:100%;margin:10px 0" frameborder=0></iframe>
```

**New pattern** (to be inserted):
```rst
#. |link_download_this_code|. After downloading, extract the zip file and open the ``<project_dir>.ino`` file in the Arduino IDE.

    .. note::

        * :ref:`unknown_com_port`
```

#### Key Rules for Multi-File Replacement

1. The `.. raw:: html` directive with the `<iframe>` is REMOVED.
2. The `.. note::` block with `:ref:\`unknown_com_port\`` is KEPT (not moved).
3. The sentence before is changed from `|link_download_this_code| or copy this code to the Arduino IDE directly.` to `|link_download_this_code|. After downloading, extract the zip file and open the \`\`<project_dir>.ino\`\` file in the Arduino IDE.`
4. No `.. code-block` is added — the code is too complex to inline.

---

### Status: All 14 files converted ✅

- **12 single-file projects**: Category A pattern (inline code) — all done.
- **2 download-only projects**: Category B pattern (download instruction) — done.
  - `ar_iot_camera_web.rst` — multi-file project
  - `ar_iot_html_cam_led.rst` — code too long to inline
- **Note**: `ar_motor.rst` had an additional iframe in its "Learn More" section (`4.1_motor_pwm.ino`, source not in copy directory) that was removed manually.

---

## 18650 Battery References Removal

Removed all references to "18650" battery model and hid the battery charging guide section.

### Changes in `component_esp32_extension.rst`

- Removed "18650" from two text descriptions (Japanese text preserved):
  - "3.7V 18650バッテリー" → "3.7V バッテリー"
  - "3.7Vの18650リチウムバッテリー" → "3.7Vのリチウムバッテリー"
- Commented out the entire **"バッテリーの電源と充電"** section using RST comments (`..`), including:
  - Battery insertion instructions
  - `plugin_battery.png` image
  - Charging instructions with `battery_charge.png` image

### Changes in `index.rst`

- Replaced `img/battery_charge.png` with `img/esp32_camera.png`
- Removed `component_battery` from the toctree

### Battery Steps Commented Out in Project Files

These battery-related steps were commented out (same as the Battery Power and Charging section):

- **`ar_iot_html_cam_led.rst`**: Commented out the battery insertion step and its `plugin_battery.png` image
- Same pattern may apply to other project files that reference battery insertion

---

## Image Alignment Cleanup (`ar_iot_blynk.rst`)

Removed excessive `:align: center` and `:width: XX%` attributes from image directives throughout the Blynk configuration tutorial. Most images now use only `:width:` (absolute pixel values like 600 or 700) without centering. This affects ~25+ image directives across sections 2.1 through 3.

### Supporting Config Changes (synced from docs-en)

- **`conf.py`**: Project name changed from "SunFounder ESP32 Starter Kit" to "SunFounder ESP32 Camera Kit"; removed `autosectionlabel` extension
- **`requirements.txt`**: Updated `sphinx_rtd_theme==3.0.2`
- **`.readthedocs.yaml`**: Added submodules config; changed formats from `all` to `epub` + `pdf` only
- **`.gitignore`**: Expanded with more patterns
