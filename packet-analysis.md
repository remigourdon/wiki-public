# Packet Analysis

## USB Sniffing

### Load usbmon

Load it with `sudo modprobe usbmon`.

Give regular users privileges with `sudo setfacl -m u:$USER:r /dev/usbmon*`.

If the kernel is prior to 2.6.23 run `mount -t debugfs none /sys/kernel/debug`.

### Find interface

Run `lsusb` to get the bus and device number.

Run `tcpdump -D` to find the interface that corresponds to that bus.

### Dump

Run `tcpdump` on the previously determined interface with `tcpdump -i usbmon2 -w ~/Capture/usbmon/usblog.pcap &`.

To stop capture, run `killall tcpdump`.

### WireShark

Then you can use [wireshark](wireshark).

Run `wireshark ~/Capture/usbmon/usblog.pcap`
