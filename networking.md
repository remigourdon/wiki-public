# Networking

## Mutually Exclusive WiFi/Ethernet

Following the `nmcli-examples` man page.

Create the file `70-wifi-wired-exclusive.sh` in `/etc/NetworkManager/dispatcher.d/` and **make it executable**:

```text
#!/bin/bash
export LC_ALL=C

enable_disable_wifi ()
{
    RESULT="$(nmcli dev | awk '$2=="ethernet" && $4=="Office" && $3=="connected" {print $1}')"
    if [ -n "${RESULT}" ]; then
        nmcli radio wifi off
    else
        nmcli radio wifi on
    fi
}

if [ "$2" = "up" ]; then
    enable_disable_wifi
fi

if [ "$2" = "down" ]; then
    enable_disable_wifi
fi
```
