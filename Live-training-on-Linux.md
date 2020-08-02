While low-level device communication is handled by the kernel, device-related events are managed in userspace by `udevd`. Custom `.rules` files can be defined in order to get access to those events without elevated privileges.In order to use live training on Linux, follow these instructions: 

In `/etc/udev/rules.d/` create a file named `50-oryx.rules`:
```bash
sudo touch /etc/udev/rules.d/50-oryx.rules
```

And paste the following configuration inside:

```conf
# Rule for the Moonlander
SUBSYSTEM=="usb", ATTR{idVendor}=="3297", ATTR{idProduct}=="1969", GROUP="plugdev"
# Rule for the Ergodox EZ
SUBSYSTEM=="usb", ATTR{idVendor}=="feed", ATTR{idProduct}=="1307", GROUP="plugdev"
# Rule for the Planck EZ
SUBSYSTEM=="usb", ATTR{idVendor}=="feed", ATTR{idProduct}=="6060", GROUP="plugdev"
```
Make sure your user is part of the plugdev group (it might not be the default on some distros):

```
$> sudo groupadd plugdev
$> sudo usermod -aG plugdev $USER
```
Make sure to logout once after that.

_Note: The snippet above defines rules for both the Ergodox EZ and the Planck EZ. Feel free to only copy the block relevant to you._

**WARNING:** On the latest Ubuntu versions and its derivatives, browsers are installed via snapd. In the Software Package manager GUI, find your browser's page and click the "Permissions button", then tick the toggle "Access USB hardware directly".