## Flashing your keyboard on ChromeOS.
Please note that these steps will work on the Planck and Moonlander only. The Ergodox bootloader advertises itself as an HID device and ChromeOS doesn't forward HID devices to the Linux container.

This will only work if your ChromeOS device [supports Linux](https://www.chromium.org/chromium-os/chrome-os-systems-supporting-linux).

Thanks to [@drafcode](https://github.com/draftcode) for the instructions.

## Steps
* Enable "Linux (Beta)" in Chrome OS settings.
* Download wally-cli and a firmware file into the Linux folder.
* chmod +x wally-cli
* ./wally-cli FIRMWARE_FILE.bin
* Press the reset button on the keyboard.
* ChromeOS shows a pop-up saying a new USB device is detected. Make it available to the Linux environment by clicking the popup or go to the settings page and allow it.
* wally-cli writes the firmware. Keyboard will be rebooted automatically.
