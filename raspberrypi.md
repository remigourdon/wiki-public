# Raspberry Pi

## USB Gadget

Append the following to `/boot/config.txt`:

```text
dtoverlay=dwc2
```

Enable SSH:

```sh
touch /boot/ssh
```

Edit `/boot/cmdline.txt`:

```text
# Add modules-load=dwc2,g_ether after rootwait
console=serial0,115200 console=tty1 root=PARTUUID=6c586e13-02 rootfstype=ext4 elevator=deadline fsck.repair=yes rootwait modules-load=dwc2,g_ether quiet init=/usr/lib/raspi-config/init_resize.sh
```

Find the network name with `ip addr show` and then in `nmtui` create an Ethernet connection for it with IPv4 in `link-local` mode.

## Check CPU Frequency

The [only way](https://github.com/raspberrypi/linux/issues/2512#issuecomment-382643327) to check for the **actual** frequency of the CPU is:

```sh
vcgencmd measure_clock arm | awk -F"=" '{printf ("%0.0f",$2/1000000); }'
```

## Connection Sharing

### Host

```sh
# Enable IP forwarding for all interfaces
sudo sysctl net.ipv4.ip_forward=1
# Enable NAT on interface that is connected to the internet
sudo iptables -t nat -A POSTROUTING -o ens1u1 -j MASQUERADE
```

### Raspberry

Add the following line to `/etc/resolv.conf` (Google's DNS):

```text
nameserver 8.8.8.8
```

Then add a route to the host:

```sh
sudo route add default gw 192.168.5.4 eth0
```

## Benchmark SD Card Speed

The Debian package `hdparm` is installed after downloading it from the repositories.

Then, it is run like this: `sudo hdparm -tT /dev/mmcblk0`.

Results without the SD card extender:

```text
/dev/mmcblk0:
 Timing cached reads:   1438 MB in  2.00 seconds = 718.71 MB/sec
 Timing buffered disk reads:  66 MB in  3.05 seconds =  21.63 MB/sec
```

Results with the SD card extender:

```text
/dev/mmcblk0:
 Timing cached reads:   1406 MB in  2.00 seconds = 702.55 MB/sec
 Timing buffered disk reads:  66 MB in  3.05 seconds =  21.63 MB/sec
```

## Cross compile for the Raspberry Pi 3

### Clone rpi-tools

```sh
git clone --depth 1 https://github.com/raspberrypi/tools rpi-tools
```

### Prepare the sysroot

This has to be done after installing the required libraries on the remote Raspberry Pi. Everytime new libraries are installed on the remote, the rsync command should be rerun.

Based on [this post](https://blog.meinside.dev/Build-Cross-Compile-Tools-for-Raspberry-Pi-on-macOS/).

```sh
mkdir -p rpi-sdk/sysroot
sudo apt install rsync
rsync -rzLR --safe-links \
	pi@192.168.4.2:/usr/lib/arm-linux-gnueabihf \
	pi@192.168.4.2:/usr/lib/gcc/arm-linux-gnueabihf \
	pi@192.168.4.2:/usr/include \
	pi@192.168.4.2:/lib/arm-linux-gnueabihf \
  pi@192.168.4.2:/usr/local/lib \
  pi@192.168.4.2:/usr/local/include \
  pi@192.168.4.2:/usr/bin \
	rpi-sdk/sysroot
  
rsync -rzLR --safe-links \
  pi@192.168.4.2:/usr/include/ \
  pi@192.168.4.2:/usr/local/include/ \
  pi@192.168.4.2:/usr/lib/arm-linux-gnueabihf/ \
  rpi-sdk/sysroot/
```

### Configure for autotools

On the **host**, install:

```sh
sudo apt install automake autotools-dev autoconf libtool
```

Based on [this post](https://sappo.github.io/2016/10/13/Cross-compiling-for-the-Raspberry-Pi/).

Create the file `rpi-sdk/autotools-config`:

```sh
TOOLCHAIN_HOST="arm-linux-gnueabihf"
TOOLCHAIN_PATH="$PWD/rpi-tools/arm-bcm2708/arm-linux-gnueabihf/bin"
CPP="${TOOLCHAIN_PATH}/${TOOLCHAIN_HOST}-cpp"
CC="${TOOLCHAIN_PATH}/${TOOLCHAIN_HOST}-gcc"
CXX="${TOOLCHAIN_PATH}/${TOOLCHAIN_HOST}-g++"
LD="${TOOLCHAIN_PATH}/${TOOLCHAIN_HOST}-ld"
AS="${TOOLCHAIN_PATH}/${TOOLCHAIN_HOST}-as"
AR="${TOOLCHAIN_PATH}/${TOOLCHAIN_HOST}-ar"
RANLIB="${TOOLCHAIN_PATH}/${TOOLCHAIN_HOST}-ranlib"

SYSROOT=$PWD/sysroot
CFLAGS+="--sysroot=${SYSROOT}"
CPPFLAGS+="--sysroot=${SYSROOT}"
CXXFLAGS+="--sysroot=${SYSROOT}"

CONFIG_OPTS=()
CONFIG_OPTS+=("CFLAGS=${CFLAGS}")
CONFIG_OPTS+=("CPPFLAGS=${CPPFLAGS}") CONFIG_OPTS+=("CXXFLAGS=${CXXFLAGS}")
CONFIG_OPTS+=("LDFLAGS=${LDFLAGS}") CONFIG_OPTS+=("PKG_CONFIG_DIR=")
CONFIG_OPTS+=("--host=${TOOLCHAIN_HOST}")
CONFIG_OPTS+=("CPP=${CPP}")
CONFIG_OPTS+=("CC=${CC}")
CONFIG_OPTS+=("CXX=${CXX}")
CONFIG_OPTS+=("LD=${LD}")
CONFIG_OPTS+=("AS=${AS}")
CONFIG_OPTS+=("AR=${AR}")
CONFIG_OPTS+=("RANLIB=${RANLIB}")
BUILD_PREFIX=$PWD/tmp
CONFIG_OPTS+=("--prefix=${BUILD_PREFIX}")
CONFIG_OPTS+=("PKG_CONFIG_DIR=")
CONFIG_OPTS+=("PKG_CONFIG_LIBDIR=${SYSROOT}/usr/lib/arm-linux-gnueabihf/pkgconfig:${SYSROOT}/usr/share/pkgconfig")
CONFIG_OPTS+=("PKG_CONFIG_SYSROOT=${SYSROOT}")
CONFIG_OPTS+=("PKG_CONFIG_PATH=${BUILD_PREFIX}/lib/pkgconfig")
```
