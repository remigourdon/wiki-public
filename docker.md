# Docker

## Docker disk usage

To properly check disk usage in `/var/lib/docker` with `du`, you need to use the `-x` option to disregard data on other filesystems.

This is needed because `/var/lib/docker/overlay2` is where Docker uses OverlayFS to give each container a read-only access to the image with its separate write overlay.
This is well explained by [Julia Evans](https://jvns.ca/blog/2019/11/18/how-containers-work--overlayfs/).

So essentially, if running without the `-x` flag, `du` will count each image `N + 1` times where `N` is the number of containers running from this image, as seen below:

```sh
$ sudo du -h --max-depth=1 /var/lib/docker
4.0K    /var/lib/docker/trust
20K     /var/lib/docker/builder
160K    /var/lib/docker/containers
72K     /var/lib/docker/network
4.0K    /var/lib/docker/swarm
20K     /var/lib/docker/plugins
4.9G    /var/lib/docker/overlay2
72K     /var/lib/docker/buildkit
7.8M    /var/lib/docker/image
4.0K    /var/lib/docker/runtimes
4.0K    /var/lib/docker/tmp
28K     /var/lib/docker/volumes
4.9G    /var/lib/docker
```

But in reality, when using the `-x` flag, we see the actual disk space used.
This corresponds to `N` images plus whatever disk space is used by each container's write overlay.

```sh
$ sudo du -xh --max-depth=1 /var/lib/docker
4.0K    /var/lib/docker/trust
20K     /var/lib/docker/builder
160K    /var/lib/docker/containers
72K     /var/lib/docker/network
4.0K    /var/lib/docker/swarm
20K     /var/lib/docker/plugins
2.1G    /var/lib/docker/overlay2
72K     /var/lib/docker/buildkit
7.8M    /var/lib/docker/image
4.0K    /var/lib/docker/runtimes
4.0K    /var/lib/docker/tmp
28K     /var/lib/docker/volumes
2.1G    /var/lib/docker
```

This is consistent with what Docker tells us (more detailed info can be obtained with the `-v` flag):

```sh
$ docker system df
TYPE                TOTAL               ACTIVE              SIZE                RECLAIMABLE
Images              5                   3                   1.957GB             1.017GB (51%)
Containers          4                   4                   1.374kB             0B (0%)
Local Volumes       0                   0                   0B                  0B
Build Cache         0                   0                   0B                  0B
```

## Log using rsyslog and logrotate

The following notes are based on [this article](https://techroads.org/docker-logging-to-the-local-os-that-works-with-compose-and-rsyslog/).

Create a template file for rsyslog, for example `30-docker.conf`.
It needs to start with a number lower than that of the `XX-default.conf` (typically `50-default.conf`) so processing runs before default and stops there (thanks to the `& ~` at the end).

```text
$template DOCKER_LOGS,"/var/log/docker/%programname%.log"

if $programname startswith  'docker-' then ?DOCKER_LOGS
& ~
```

Install this configuration file and restart rsyslog:

```sh
sudo install -m 0644 30-docker.conf /etc/rsyslog.d/
sudo systemctl restart rsyslog
```

Create the directory to old the container logs:

```sh
sudo mkdir /var/log/docker
sudo chown syslog:adm /var/log/docker
sudo chmod 2775 /var/log/docker
```

Create a logrotate configuration called `docker`:

```text
/var/log/docker/*.log {
        create 0640 syslog adm
        rotate 7
        daily
        compress
        missingok
}
```

Install this configuration file and restart logrotate:

```sh
sudo install -m 0644 docker /etc/logrotate.d/
sudo systemctl restart logrotate
```

