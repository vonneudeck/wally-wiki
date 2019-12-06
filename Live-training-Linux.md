While low-level device communication is handled by the kernel, device-related events are managed in userspace by `udevd`. Custom `.rules` files can be defined in order to get access to those events without elevated privileges.

In `/etc/udev/rules.d/` create a file named `50-oryx.rules`:
```bash
sudo touch /etc/udev/rules.d/50-oryx.rules
```

And paste the following configuration inside:

```conf
# Rule for the Ergodox EZ Original / Shine / Glow
SUBSYSTEM=="usb", ATTR{idVendor}=="FEED", ATTR{idProduct}=="1307", GROUP="plugdev"
# Rule for the Planck EZ Standard / Glow
SUBSYSTEM=="usb", ATTR{idVendor}=="FEED", ATTR{idProduct}=="6060", GROUP="plugdev"
```

_Note: The snippet above defines rules for both the Ergodox EZ and the Planck EZ. Feel free to only copy the block relevant to you._