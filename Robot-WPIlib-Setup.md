## Robot WPIlib Setup

Make sure you have git installed.

```
sudo pacman -S git konsole
```

Install a default font

```
sudo pacman -S noto-fonts
```

Clone the common git repositories (repos).

```
cd ~
mkdir -p ~/git/OysterRiverOverdrive

git clone https://github.com/OysterRiverOverdrive/Charged-Up-2023-Atlas.git ~/git/OysterRiverOverdrive/Charged-Up-2023-Atlas
```

Install WPILib

```
curl -LO https://github.com/wpilibsuite/allwpilib/releases/download/v2023.1.1/WPILib_Linux-2023.1.1.tar.gz

tar -xf WPILib_Linux-2023.1.1.tar.gz
cd WPILib_Linux-2023.1.1/
./WPILibInstaller # run from the laptop
```


WPILib Install Options:

* Install "Everything"
* Install for this User
* Download for this computer only
