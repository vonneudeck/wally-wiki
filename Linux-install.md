In order to use Wally and flash your board on Linux, you need to:

1. Install the required dependencies.
2. Add a udev rule file to your distro.
3. Download the Wally binary.

## 1. Install the required dependencies.

Note some distributions might already have these dependencies installed (ex: Ubuntu)

Wally's GUI requires three dependencies to run properly: `gtk3`, `webkit2gtk` and `libusb`.
If you plan to use the CLI version only, then only `libusb` is required.

### 1.1 Arch and its derivatives (Manjaro, Antergox, ...)
```bash
sudo pacman -S gtk3 webkit2gtk libusb
```

### 1.2 Debian and its derivatives (Ubuntu, Kali, ...)
```bash
sudo apt-get install gtk+3.0 webkit2gtk-4.0 libusb-dev
```

### 1.3 Red Hat / Fedora and its derivatives (Centos, Scientific Linux, ...)
```bash
sudo yum install gtk3 webkit2gtk3 libusb
```

_Note: Feel free to open a PR with an update to the docs if you would like your distro listed above._

## 2. Create a udev rule file
While low-level device communication is handled by the kernel, device-related events are managed in userspace by `udevd`. Custom `.rules` files can be defined in order to get access to those events without elevated privileges.

In `/etc/udev/rules.d/` create a file named `50-wally.rules`:
```bash
sudo touch /etc/udev/rules.d/50-wally.rules
```

And paste the following configuration inside:

```conf
# Teensy rules for the Ergodox EZ Original / Shine / Glow
ATTRS{idVendor}=="16c0", ATTRS{idProduct}=="04[789B]?", ENV{ID_MM_DEVICE_IGNORE}="1"
ATTRS{idVendor}=="16c0", ATTRS{idProduct}=="04[789A]?", ENV{MTP_NO_PROBE}="1"
SUBSYSTEMS=="usb", ATTRS{idVendor}=="16c0", ATTRS{idProduct}=="04[789ABCD]?", MODE:="0666"
KERNEL=="ttyACM*", ATTRS{idVendor}=="16c0", ATTRS{idProduct}=="04[789B]?", MODE:="0666"

# STM32 rules for the Planck EZ Standard / Glow
SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="df11", \
    MODE:="0666", \
    SYMLINK+="stm32_dfu"
```

_Note: The snippet above defines rules for both the Ergodox EZ and the Planck EZ. Feel free to only copy the block relevant to you._

## 3. Download the Wally binary and run it

Download the latest "Linux 64" version from the [release page](https://github.com/zsa/wally/releases), make it executable (`chmod +x wally`) and execute it.

Ran into an issue while following this guide? Feel free to [contact us](mailto:contact@ergodox-ez.com) and we'll help!