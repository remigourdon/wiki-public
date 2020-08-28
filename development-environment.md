# Development environment

## Oh-My-Zsh

https://github.com/ohmyzsh/ohmyzsh

### Ubuntu

```sh
sudo apt install zsh
chsh -s /bin/zsh
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### Arch Linux

```sh
yay -S zsh
chsh -s /bin/zsh
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## Neovim

### Ubuntu

`neovim` is available in the repositories, but the version is quite old.
For Ubuntu up to 18.04, it can be installed using a PPA.

```sh
sudo add-apt-repository ppa:neovim-ppa/stable # Stable
sudo add-apt-repository ppa:neovim-ppa/unstable # Unstable
sudo apt update
sudo apt install neovim
```

### Arch Linux

```sh
yay -S neovim
```

## Tmux

https://github.com/tmux/tmux

### Ubuntu

The latest AppImage can be [downloaded here](https://github.com/tmux/tmux/releases) or install from source as follows:

```sh
sudo apt install libncurses5-dev
git clone https://github.com/tmux/tmux.git
cd tmux
sh autogen.sh
./configure && make
```

### Arch Linux

```sh
yay -S tmux
```

## Rust

https://github.com/rust-lang

### Ubuntu

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### Arch Linux

```sh
yay -S rustup
```

### Completions

```sh
rustup completions zsh cargo > ~/.zsh_completions/_cargo
rm .zcompdump*
```

## Go

### Ubuntu

```sh
sudo snap install go --classic
```

### Arch Linux

```sh
yay -S go
```

## NodeJS

To install latest do the following.
Another option is to use the [Node Version Manager](https://github.com/nvm-sh/nvm#installing-and-updating).

### Ubuntu

```sh
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt install nodejs
```

### Arch Linux

```sh
yay -S nodejs
```

## Exercism.io

CLI client to download exercises and submit solutions [the Exercism platform](https://exercism.io/).

### Ubuntu

```sh
sudo snap install exercism
```

### Arch Linux

```sh
yay -S exercism-bin
```

## Tig (git CLI interface)

https://github.com/jonas/tig

### Ubuntu

```sh
sudo apt install tig
```

### Arch Linux

```sh
yay -S tig
```

## Quilt (manage patches in series)

https://linux.die.net/man/1/quilt

This is very useful to build patch series for Buildroot for example.

A good tutorial to start is [this one](https://wiki.debian.org/UsingQuilt).

### Ubuntu

```sh
sudo apt install quilt
```

### Arch Linux

```sh
yay -S quilt
```

## Shellcheck (shell script static analysis)

https://github.com/koalaman/shellcheck

Very, very valuable tool to check shell scripts for common mistakes and best practices.

### Ubuntu

```sh
sudo apt install shellcheck
```

### Arch Linux

```sh
yay -S shellcheck
```

## Julia

https://github.com/JuliaLang/

### Ubuntu

Best way to get a recent version is to download the binary directly and symlink it to a directory in the `PATH`.

```sh
wget https://julialang-s3.julialang.org/bin/linux/x64/1.5/julia-1.5.1-linux-x86_64.tar.gz
tar zxvf julia-1.5.1-linux-x86_64.tar.gz
ln -s julia-1.5.1/bin/julia ~/.local/bin/
```

### Arch Linux

```sh
yay -S julia
```

## Pluto (Julia-based reactive notebook)

https://github.com/fonsp/Pluto.jl

Install the package directly in Julia and start the server:

```jl
julia> ]
(v1.0) pkg> add Pluto
julia> import Pluto
julia> Pluto.run(1234) # Start server on http://localhost:1234
```
