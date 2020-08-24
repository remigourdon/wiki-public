# Arch Linux install

## LVM on LUKS

Following [this](https://www.coded-with-love.com/blog/install-arch-linux-encrypted/) for partitioning and [this](https://wiki.archlinux.org/index.php/Dm-crypt/Encrypting_an_entire_system#LVM_on_LUKS) for encryption.

The disk is wiped after partitioning following [dm-crypt specific methods](https://wiki.archlinux.org/index.php/Dm-crypt/Drive_preparation).

Connect to WiFi with:

```sh
iwctl
device list
station wlan0 connect YOUR-SSID
```

Partition the disk:

1. Partition is EFI (code `ef00` and size 512M)
2. Partition is LUKS (code `8309` and remaining space)

```sh
cryptsetup luksFormat /dev/nvme0n1p2 # Format partition to be LUKS and choose passphrase
cryptsetup open /dev/nvme0n1p2 cryptlvm # Open encrypted partition
pvcreate /dev/mapper/cryptlvm # Create physical volume on top of opened LUKS container
vgcreate main /dev/mapper/cryptlvm # Create volume group named main

# Create logical volumes on the volume group
lvcreate -L 8G main -n swap
lvcreate -L 40G main -n root
lvcreate -l 100%FREE main -n home

# Format filesystems on each logical volume
mkfs.ext4 /dev/main/root
mkfs.ext4 /dev/main/home
mkswap /dev/main/swap

# Mount filesystems
mount /dev/main/root /mnt
mkdir /mnt/home
mount /dev/main/home /mnt/home
swapon /dev/main/swap

mkfs.fat -F32 /dev/nvme0n1p1 # Create filesystem on boot partition
mkdir /mnt/boot
mount /dev/nvme0n1p1 /mnt/boot

# Install basics to new environment
pacstrap /mnt base base-devel linux linux-firmware man-db man-pages texinfo vim

genfstab -U /mnt >> /mnt/etc/fstab # Generate fstab

arch-chroot /mnt # chroot into new environment

# Set timezone and hardware clock
ln -sf /usr/share/zoneinfo/America/New_York /etc/localtime
hwclock --systohc
```

Edit `/etc/locale.gen` and uncomment useful locales then run `locale-gen`.

In `/etc/locale.conf` set:

```text
LANG=en_US.UTF-8
```

Set desired hostname in `/etc/hostname` and edit `/etc/hosts` accordingly:

```text
127.0.0.1	localhost
::1		localhost
127.0.1.1	myhostname.localdomain	myhostname
```

Setup the hooks in `/etc/mkinitcpio.conf`:

```text
HOOKS=(base udev autodetect keyboard keymap consolefont modconf block encrypt lvm2 filesystems fsck)
```

Create the initramfs:

```sh
pacman -S lvm2 # Install the lvm2 package
mkinitcpio -P
```

Install network manager:

```sh
pacman -S networkmanager
```

Install the systemd bootloader:

```sh
bootctl --path=/boot/ install
```

Edit the bootloader configuration file `/boot/loader/loader.conf`:

```text
default arch
editor 0
```

Create the Arch Linux specific bootloader configuration `/boot/loader/entries/arch.conf`:

```text
title Arch Linux
linux /vmlinuz-linux
initrd  /initramfs-linux.img
options cryptdevice=/dev/nvme0n1p2:main root=/dev/main/root resume=/dev/main/swap lang=en locale=en_US.UTF-8
```

Define root password with `passwd`.

Exit chroot and reboot:

```sh
exit
umount -R /mnt
reboot
```

## Post-installation

### Add new user

```sh
useradd -m USER
passwd USER
```

### Privilege escalation

Add new user to wheel group:

```sh
usermod -aG wheel USER
```

Run `visudo` and uncomment the line:

```text
%wheel ALL=(ALL) ALL
```

Then **change** to new user!

### Network Manager

```sh
systemctl enable NetworkManager.service
systemctl start NetworkManager.service
ip link show # find interface name
nmtui
```

### User directories

```sh
pacman -S xdg-user-dirs
xdg-user-dirs-update
```

### yay

```sh
pacman -S git openssh
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

### st

```sh
git clone git@github.com:remigourdon/st.git
```

### i3

```sh
yay -S xorg-server xorg-xwininfo xorg-xinit xorg-xprop \
xorg-xdpyinfo xorg-xbacklight xorg-xev xwallpaper xclip \
i3-gaps noto-fonts xf86-input-libinput xorg-xinput polybar \
gnome-keyring udiskie
```

### Audio

```sh
yay -S pulseaudio pulseaudio-alsa pulsemixer
```

### Touchpad

Using the `xf86-input-libinput` driver and `xorg-xinput`, check the devices available by running `xinput list`.

Then it is possible to test properties by running `xinput set-prop DEVICE_ID PROP_ID VALUE`.

To set configuration permanently, create `/etc/X11/xorg.conf.d/30-touchpad.conf` containing and reboot:

```text
Section "InputClass"
    Identifier "touchpad"
    Driver "libinput"
    MatchIsTouchpad "on"
    Option "DisableWhileTyping" "true"
    Option "NaturalScrolling" "true"
    Option "ScrollMethod" "twofinger"
    Option "Tapping" "true"
    Option "TappingButtonMap" "lrm"
    Option "TappingDrag" "true"
    Option "TappingDragLock" "false"
EndSection
```
