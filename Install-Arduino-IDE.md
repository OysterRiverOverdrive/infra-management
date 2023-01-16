## Install Arduino IDE

Verify USB port access has been established.


* A file managed should be installed like `dolphin`.  Without it the device won't show up on the list of ports in the Arduino IDE.  If needed, `sudo pacman -S dolphin`.
* Make sure the user is part of the uucp group to be able to access the USB port.

```
sudo usermod -a -G uucp dan
# You may need to log out and in again or restart.
# `id [username]` should show the new group added.
```

Install the arduino IDE and command line and install a list of common Arduino boards.

```
sudo pacman -S arduino arduino-cli

arduino-cli -v core install arduino:avr
# Okay to ignore errors like "A new release of Arduino CLI is available:" for now.
```
