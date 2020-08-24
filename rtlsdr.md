# RTL-SDR

## Installation

```sh
sudo apt-get install cmake
sudo apt-get install git
sudo git clone git://git.osmocom.org/rtl-sdr.git
cd rtl-sdr/
mkdir build
cd build
cmake ../
make
sudo make install
sudo ldconfig
cmake ../ -DINSTALL_UDEV_RULES=ON
```

## Blacklist

If errors as following:

> Kernel driver is active, or device is claimed by second instance of librtlsdr.
> In the first case, please either detach or blacklist the kernel module
> (dvb_usb_rtl28xxu), or enable automatic detaching at compile time.

Create a file `/etc/modprobe.d/ban-rtl.conf` containing:

```text
blacklist dvb_usb_rtl28xxu
blacklist dvb_usb_v2
blacklist rtl_2830
blacklist rtl_2832
blacklist r820t
```

## Issues

The onboard tuner **Fitipower FC0012** has a gap in RX between 948.6 MHz and approximately 1400MHz.

## GQRX

Perform the following to build from sources:

```sh
sudo apt-get install qt5-default libqt5svg5-dev libpulse-dev
git clone https://github.com/csete/gqrx.git
cd gqrx
mkdir build
cd build/
cmake ..
```
