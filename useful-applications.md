# Useful applications

## Data manipulation

### ripgrep (alternative to grep)

https://github.com/BurntSushi/ripgrep

#### Ubuntu

```sh
wget https://github.com/BurntSushi/ripgrep/releases/download/11.0.2/ripgrep_11.0.2_amd64.deb
sudo dpkg -i ripgrep_11.0.2_amd64.deb
```

#### Arch Linux

```sh
yay -S ripgrep
```

### Ultimate Plumber

https://github.com/akavel/up

This is a great tool to interactively build up text processing pipelines.

```sh
go get -u github.com/akavel/up
```

#### Arch Linux

```sh
yay -S up
```

### q (CLI tool to run SQL on CSV/TSV)

https://github.com/harelba/q

#### Ubuntu

It is released as a Debian package:

```sh
wget https://github.com/harelba/packages-for-q/raw/master/deb/q-text-as-data_2.0.9-2_amd64.deb
https://github.com/harelba/packages-for-q/raw/master/deb/q-text-as-data_2.0.9-2_amd64.deb
```

#### Arch Linux

```sh
yay -S q
```

### xsv (CSV command-line toolkit)

https://github.com/BurntSushi/xsv

```sh
cargo install xsv
```

### hexyl (improved CLI hex viewer)

https://github.com/sharkdp/hexyl

#### Ubuntu

```sh
sudo snap install hexyl
```

#### Arch Linux

```sh
yay -S hexyl
```

## Filesystem

### lf (file manager)

https://github.com/gokcehan/lf

```sh
go get -u github.com/gokcehan/lf
```

#### Arch Linux

```sh
yay -S lf
```

### z (directories frecency tracker)

https://github.com/rupa/z

This is similar to `fasd` but limited to directories.

#### Ubuntu

```sh
sudo apt install z
```

#### Arch Linux

```sh
yay -S z
```

### fd (alternative to find)

https://github.com/sharkdp/fd

#### Ubuntu

```sh
wget https://github.com/sharkdp/fd/releases/download/v7.5.0/fd-musl_7.5.0_amd64.deb
sudo dpkg -i fd-musl_7.5.0_amd64.deb
```

#### Arch Linux

```sh
yay -S fd
```

### udiskie (auto-mounter for removable media)

https://github.com/coldfix/udiskie

This is great to auto-mount USB sticks, SD cards, etc.
It also provides a CLI to easily unmount devices.

#### Ubuntu

```sh
sudo apt install udiskie
```

#### Arch Linux

```sh
yay -S udiskie
```

### archivemount (mounter for compressed archive files)

https://linux.die.net/man/1/archivemount

#### Ubuntu

```sh
sudo apt install archivemount
```

#### Arch Linux

```sh
yay -S archivemount
```

### dua (du replacement)

https://github.com/Byron/dua-cli

```sh
cargo install dua-cli
```

#### Arch Linux

```sh
yay -S dua-cli
```

### exa (ls replacement)

https://github.com/ogham/exa

```sh
cargo install exa
```

#### Arch Linux

```sh
yay -S exa
```

### lnav (log file navigation)

https://github.com/tstack/lnav

Great tool for interactive exploration of log files.

#### Ubuntu

```sh
sudo apt install lnav
```

#### Arch Linux

```sh
yay -S lnav
```

## Containerization / Virtualization

### Dive (docker layers explorer)

https://github.com/wagoodman/dive

This is a neat tool to explore each layer, as in seeing the fiiesystem content at that layer's level.
It's useful to optimize the images but also generally to understand how Docker images are built.

#### Ubuntu

```sh
wget https://github.com/wagoodman/dive/releases/download/v0.9.2/dive_0.9.2_linux_amd64.deb
sudo dpkg -i dive_0.9.2_linux_amd64.deb
```

#### Arch Linux

```sh
yay -S dive
```

## Configuration management

### chezmoi (dotfiles management system)

https://github.com/twpayne/chezmoi

Currently used with `bitwarden` for secrets.

```sh
chezmoi init https://github.com/remigourdon/dotfiles.git
```

#### Ubuntu

```sh
snap install bw
snap install chezmoi --classic
```

#### Arch Linux

```sh
yay -S bitwarden-cli
yay -S chezmoi
```

### navi (interactive cheatsheet tool)

https://github.com/denisidoro/navi

My cheatsheets (based on the provided defaults) are available [here](https://github.com/remigourdon/cheats.git)

```sh
git clone https://github.com/denisidoro/navi
cd ~/.navi
make BIN_DIR=~/.local/bin/ install
```

#### Arch Linux

```sh
yay -S navi
```

### delta (viewer for diff and git diff)

https://github.com/dandavison/delta

#### Ubuntu

Download binary from [release page](https://github.com/dandavison/delta/releases).

#### Arch Linux

```sh
yay -S git-delta
yay -S git-delta-bin
```

## Network

### HTTPie (CLI interface to test HTTP APIs)

https://github.com/httpie/httpie

#### Ubuntu

```sh
sudo apt install httpie
```

#### Arch Linux

```sh
yay -S httpie
```

### mosquitto (MQTT broker)

https://github.com/eclipse/mosquitto

#### Ubuntu

There is a [PPA](https://launchpad.net/~mosquitto-dev/+archive/ubuntu/mosquitto-ppa) available but the version in the repositories seemed recent enough.

The `mosquitto-clients` package provides the `mosquitto_sub` and `mosquitto_pub` CLI clients to respectively subscribe and publish on MQTT topics.

```sh
sudo apt install mosquitto mosquitto-clients
```

#### Arch Linux

```sh
yay -S mosquitto
```


### websocat (netcat equivalent for Websockets)

https://github.com/vi/websocat

```sh
cargo install --features=ssl websocat
```

### RustScan (nmap complement)

https://github.com/RustScan/RustScan

```sh
cargo install rustscan
```

#### Arch Linux

```sh
yay -S rustscan
```

## Document viewer

### bat (improved cat)

https://github.com/sharkdp/bat

#### Ubuntu

```sh
wget https://github.com/sharkdp/bat/releases/download/v0.12.1/bat_0.12.1_amd64.deb
sudo dpkg -i bat_0.12.1_amd64.deb
```

#### Arch Linux

```sh
yay -S bat
```

### Zathura (PDF viewer)

https://git.pwmt.org/pwmt/zathura

#### Ubuntu

```sh
sudo apt install zathura
```

#### Arch Linux

```sh
yay -S zathura zathura-pdf-poppler
```

### sxiv (image viewer)

https://github.com/muennich/sxiv

#### Ubuntu

```sh
sudo apt install sxiv
```

#### Arch Linux

```sh
yay -S sxiv
```

## Miscellaneous

### moreutils (extra UNIX utilities)

https://joeyh.name/code/moreutils/

Specifically use it for `vidir` (to batch rename files using a text editor) and parallel (to run multiple jobs at once).

#### Ubuntu

```sh
sudo apt install moreutils
```

#### Arch Linux

```sh
yay -S moreutils
```

### Tio (TTY terminal)

https://github.com/tio/tio

Very simply terminal application, typically to connect to hardware over UART.
This is similar to minicom but is even simpler to use.

#### Ubuntu

```sh
sudo apt install tio
```

#### Arch Linux

```sh
yay -S tio
```

### Draw.io (diagraming software)

This can be used in the browser but there is also an Electron-based version [here](https://github.com/jgraph/drawio-desktop).

The easiest is to simply [download the latest AppImage](https://github.com/jgraph/drawio-desktop/releases).
