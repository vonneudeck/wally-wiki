In order to use Wally and flash your board on Linux, you need to:

1. Install the required dependencies.
2. Add a udev rule file to your distro.
3. Download the Wally binary.

## 1. Install the required dependencies.

Note some distributions might already have these dependencies installed (ex: Ubuntu, Fedora Silverblue 33)

Wally's GUI requires three dependencies to run properly: `gtk3`, `webkit2gtk` and `libusb`.
If you plan to use the CLI version only, then only `libusb` is required.

### 1.1 Arch and its derivatives (Manjaro, Antergos, ...)
Note: There is an [Aur Package](https://aur.archlinux.org/packages/zsa-wally/) available now, courtesy of @synaptiko
```bash
sudo pacman -S gtk3 webkit2gtk libusb
```

### 1.2 Debian and its derivatives (Ubuntu, Kali, Mint, ...)
```bash
sudo apt install libusb-dev
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
# Rule for the Moonlander
SUBSYSTEM=="usb", ATTR{idVendor}=="3297", ATTR{idProduct}=="1969", TAG+="uaccess", TAG+="udev-acl"
# Rule for the Ergodox EZ, Planck EZ
SUBSYSTEM=="usb", ATTR{idVendor}=="feed", ATTR{idProduct}=="1307|6060", TAG+="uaccess", TAG+="udev-acl"
```

Some (e.g. Fedora Silverblue 33) but not all distributions need this additional rule when flashing the Moolander or Planck EZ:

```conf
# STM32 rules for the Moonlander and Planck EZ
SUBSYSTEM=="usb", ATTR{idVendor}=="0483", ATTR{idProduct}=="df11", TAG+="uaccess", TAG+="udev-acl"
```

_Note: The snippet above defines rules for all ZSA's keyboards. Feel free to only copy the block relevant to you._

Restart udev so that the new rules are loaded:

`sudo udevadm control --reload-rules && sudo udevadm trigger`

Finally, unplug and plug back in the keyboard to ensure that the new rules take effect.

## 3. Download the Wally binary and run it

Download the [latest linux version available from here](https://configure.ergodox-ez.com/wally/linux), make it executable (`chmod +x wally`) and execute it with `./wally`

If the internal log in the app (click on the little terminal logo in lower left to see it) reads `OpenDevices: libusb: bad access [code -3]` you forgot to use `sudo` to run wally.

Ran into an issue while following this guide? Feel free to [contact us](mailto:contact@ergodox-ez.com) and we'll help!