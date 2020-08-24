# Window manager

I use [i3wm](https://i3wm.org/), a tiling window manager.
The [i3-gaps](https://github.com/Airblader/i3) version is even better looking, and [i3lock-fancy](https://github.com/meskarune/i3lock-fancy) makes for a slick lock screen!
My configuration is [in my dotfiles](https://github.com/remigourdon/dotfiles/blob/master/private_dot_config/i3/config).

I use it along with [polybar](https://github.com/polybar/polybar), for which you can find my configurations and custom scripts [here](https://github.com/remigourdon/dotfiles/tree/master/private_dot_config/polybar) and [dunst](https://github.com/dunst-project/dunst) as the notification daemon.

Additionally, I use [pywal](https://github.com/dylanaraps/pywal) to manage the theme (across applications), along with [feh](https://github.com/derf/feh) for wallpaper setup.
See [this script](https://github.com/remigourdon/scripts/blob/master/%2Cwallpaper-set) and [templates](https://github.com/remigourdon/dotfiles/tree/master/private_dot_config/wal/templates) (explained [here](https://github.com/dylanaraps/pywal/wiki/User-Template-Files)).

Finally, [rofi](https://github.com/davatorium/rofi) is used as a dmenu replacement for user interaction, with the configuration and custom scripts available [here](https://github.com/remigourdon/dotfiles/tree/master/private_dot_config/rofi).

## Base installation

### Ubuntu

On Ubuntu, you need a PPA.

```sh
sudo add-apt-repository ppa:kgilmer/speed-ricer
sudo apt update
sudo apt install i3-gaps-wm i3lock-fancy polybar pywal feh rofi dunst
```

### Arch Linux

```sh
yay -S i3-gaps i3lock-fancy-git polybar python-pywal feh rofi dunst
```

## X11 programs

[arandr](https://christian.amsuess.com/tools/arandr/) is used as a GUI for XRandR to manage multiple screens.

[xclip](https://github.com/astrand/xclip) is used to manager the clipboard through a CLI.

[xss-lock](https://www.mankier.com/1/xss-lock) enables the use of an external locker for the screensaver (in this case i3lock-fancy).

### Ubuntu

```sh
sudo apt install arandr xclip xss-lock
```

### Arch Linux

```sh
yay -S arandr xclip xss-lock
```
