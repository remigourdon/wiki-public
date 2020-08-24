# Wireshark

[Sample captures](https://wiki.wireshark.org/SampleCaptures)

## High level functioning

1. Tap in the network at the correct location
2. Gather from network (libpcap, airpcap, winpcap)
3. Capture (eventually with capture filters)
4. Decode with EPAN (Etheral Packet ANalyze)
    1. protocol tree
    2. dissector (and dissector plugins)
    3. display filters

## [USB Capture](https://wiki.wireshark.org/CaptureSetup/USB)

```sh
sudo setfacl -m u:<USER>:r /dev/usbmon*
```

## tshark

When using `tshark` to postprocess a pcapng file, it might be needed to use the `-2` option for two-pass analysis.
This is for example needed for `gsm_sms` data which is reconstructed from multiple fragments.
Without it the output data packet is broken.
