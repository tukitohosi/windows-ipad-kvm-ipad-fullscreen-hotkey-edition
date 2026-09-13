# Windows iPad KVM — iPad Fullscreen Hotkey Edition

English | [简体中文](README.zh-CN.md)

Share one Windows mouse and keyboard with an iPad through an ESP32-C3 BLE HID bridge. Move the pointer through the right edge of Windows to control the iPad. This edition does not perform automatic left-edge return, so the entire iPad screen remains reachable; press `Ctrl+Alt+Esc` to return to Windows.

`Windows input -> Python host bridge -> USB serial -> ESP32-C3 -> Bluetooth HID -> iPad`

## Features

- Enters iPad control from the right edge of the Windows desktop.
- Keeps the full iPad screen available without an automatic left-edge exit zone.
- Returns with `Ctrl+Alt+Esc`; `Scroll Lock` is also available as a secondary control.
- Forwards mouse movement, five mouse buttons, wheel input, and keyboard reports.
- Maps Windows/GUI and Alt keys to the corresponding iPad Command and Option modifiers.
- Refuses remote mode while the iPad is disconnected and releases Windows input after a USB/Bluetooth or capture failure.
- Requires no resident iPad app and no cloud service.

## Requirements

- Windows 10 or Windows 11.
- Python 3.11 or newer.
- ESP32-C3 board with a data-capable USB cable.
- iPadOS 13.4 or newer.
- PlatformIO for building and flashing the firmware.

## Install and use

1. Install the firmware tool and build the ESP32-C3 source:

   ```powershell
   py -m pip install platformio
   cd firmware
   pio run
   pio run --target upload --upload-port COM3
   cd ..
   ```

2. In iPad Bluetooth settings, pair with `MouseLink-iPad`.
3. Create the Windows Python environment:

   ```powershell
   py -m venv .venv
   .\.venv\Scripts\python.exe -m pip install -r requirements.txt
   ```

4. Start the bridge:

   ```powershell
   .\.venv\Scripts\python.exe host\bridge.py
   ```

5. Move the mouse to the Windows desktop's right edge to enter iPad mode. Press `Ctrl+Alt+Esc` to return to Windows. `Scroll Lock` remains available where the keyboard exposes it as a distinct key.

The program automatically looks for an Espressif serial device. If the board is not detected, verify the USB cable, COM port, firmware, and iPad Bluetooth connection.

## Important behavior

- This edition intentionally has no automatic iPad left-edge return.
- Windows input is suppressed only while iPad mode is active.
- Chinese text is entered through the iPad's own input method. Windows IME composition is not transferred directly.
- Display streaming, file drag-and-drop, and cross-device clipboard synchronization are outside this project's scope.
- Keep `Ctrl+Alt+Esc` available as the primary way to return to Windows.

## Source layout

- `host/bridge.py` — Windows input router and safety recovery.
- `firmware/` — ESP32-C3 BLE HID firmware and PlatformIO configuration.
- `tools/hid_diagnostic.py` — isolated HID diagnostic helper.

## License and attribution

This project is derived from [KMChris/esp32-kvm](https://github.com/KMChris/esp32-kvm), pinned during development at commit `99c52bc36128867d2a7ed85417c1043a7d06ed5c`. See [LICENSE](LICENSE) and [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

