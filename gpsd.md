# GPS Daemon

## Install gpsd

```sh
sudo apt-get update
sudo apt-get install gpsd gpsd-clients
```

## gpsfake

Generate fake NMEA data using this [NMEA Generator](https://nmeagen.org/).

Stop the running `gpsd` daemon and start the `gpsfake` instance (which runs a daemon itself):

```sh
sudo systemctl stop gpsd.socket
gpsfake -c 1 -P 2947 fake.nmea
```

## Get time and lat/lon in CLI

```sh
gpspipe -w | sed -n 's/.*"time":"\([^"]\+\)".*"lat":\([^,]\+\),"lon":\([^,]\+\).*/\1,\2,\3/p'
```
