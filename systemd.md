# systemd

## Logging to Dedicated File

For each service, create a `rsyslog` configuration in `/etc/rsyslog.d/FILE.conf` and give it `644` permissions:

```text
if $programname == 'PROGRAM_ID' then /path/to/log/file.log
& stop
```

For multiple services starting with same name:

```text
if $programname startswith 'PROGRAM_ID' then /path/to/log/file.log
& stop
```

In the service file itself, add:

```text
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=PROGRAM_ID
```

Finally, run `sudo systemctl restart rsyslog`.
