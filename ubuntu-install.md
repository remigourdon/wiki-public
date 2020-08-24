# Ubuntu install

Notes on installation of Ubuntu on Thinkpad Carbon 6th Gen.

## Xsession

Copy `/usr/share/xsessions/i3.desktop` to `/usr/share/xsessions/custom.desktop` and add:

```text
[Desktop Entry]
Name=custom
Comment=improved dynamic tiling window manager
Exec=/etc/X11/Xsession
Type=Application
X-LightDM-DesktopName=i3
DesktopNames=i3
Keywords=tiling;wm;windowmanager;window;manager;
```

## USB-C Dock

Create `/usr/share/X11/xorg.conf.d/20-displaylink.conf`:

```text
Section "Device"
    Identifier "DisplayLink"
    Driver "modesetting"
    Option "PageFlip" "false"
EndSection
```

```sh
boltctl list
boltctl enroll 00cd2054-ef95-0801-ffff-ffffffffffff
```

## Backlight

```sh
sudo usermod -a -G video remi
sudo apt install brightnessctl
```

Create `/usr/share/X11/xorg.conf.d/20-intel.conf` then log out and back in:

```text
Section "Device"
        Identifier  "card0"
        Driver      "intel"
        Option      "Backlight"  "intel_backlight"
        BusID       "PCI:0:2:0"
EndSection
```

## Trackpad

```sh
sudo apt install xserver-xorg-input-synaptics-hwe-18.04
sudo mkdir /etc/X11/xorg.conf.d/
sudo cp .dotfiles/gui/x11/30-touchpad.conf /etc/X11/xorg.conf.d/
```

Also created the script `~/.local/bin/mousefix.sh` to be run when the trackpad seems to behave:

```sh
#!/bin/bash
# Quick fix for when the trackpad behaves
set -u # Treat unset variables and parameters as errors

sudo modprobe -r psmouse
sudo modprobe psmouse
```
