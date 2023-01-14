## Install GUI

Update the package list.

```
sudo pacman -Syyu
```

Install driver and utilities for the HP EliteBook

(See the appropriate packages to install from https://wiki.archlinux.org/title/Xorg#Driver_installation)

Note: The HP EliteBook has an i5-6300U CPU (see `lscpu`) which is generation 6 so install `mesa-amber` instead of `mesa`

```
sudo pacman -S xf86-video-intel mesa-amber
```

Install Xorg (accept defaults)

```
sudo pacman -S xorg xterm xorg-xinit
```

Install Cinnamon desktop environment

```
sudo pacman -S cinnamon nemo-fileroller
```

Install a Display Manager (accept defaults)

```
sudo pacman -S sddm
sudo systemctl enable sddm
sudo systemctl start sddm
```
