## Wipe a laptop clean and (re-)install linux

curl -LO https://mirror.clarkson.edu/archlinux/iso/2023.01.01/archlinux-2023.01.01-x86_64.iso

shasum -a 256 archlinux-2023.01.01-x86_64.iso

compare against checksum from https://archlinux.org/download/

use lsblk to find usb drive

sudo dd if=archlinux-2023.01.01-x86_64.iso of=/dev/sda status=progress

Boot from USB

Standard/default installation option


HP EliteBook

Boot
F12 to enter the network boot menu
esc
bios setup
Advanced
Boot Options
* Make sure USB Storage Boot is checked. if not check and save
Secure Boot Configuration
* Legacy Support Disable and Secure Boot Disable

Plug USB into right USB port

exit BIos menu and save changes

restart and f12
boot menu
UEFI - (USB Device)

Opens GNU Grub Menu
Select "Arch Linux install medium (x86_64, UEFI)"
Enter

Confirm booted into UEFI mode if files are present in
ls /sys/firmware/efi/efivars
If not, return to bios boot options

connect an ethernet cord or set up wifi with
iwctl
Ensure you have a wireless device available
device list
Connect to the wifi
station wlan0 connect [WiFi SSID]
Enter the wifi password if required

Exit the iwctl menu
exit

Verify you're connected to the wifi.  The wireless network interface should show as "UP".
ip link

Synchronize the system clock
timedatectl set-ntp true

cfdisk

if converting from a windows machine, you'd see
/dev/sda1 .... EFI System
/dev/sda2 .... Microsoft reserved
/dev/sda3 .... Microsoft basic data
/dev/sda4 .... Windows recovery environment
Free Space ........

Delete all /dev/sda#

Should see
Free Space ..... (size of hard drive)

New -> size 600MB
Type -> EFI System

Scroll down to Free Space

New -> size 1G
Type -> Linux swap

Scroll down to Free Space

New -> (just enter to accept the remains of the free space)
Type -> Linux filesystem

Confirm the 3 entries look correct

/dev/sda1 .... EFI System
/dev/sda2 .... Linux swap
/dev/sda3 .... Linux filesystem

Write
Are you sure you want to write the partition table to disk? yes

quit

Confirm the block storage has been set up and the 3 partitions show on sda
lsblk

Set up the EFI system partition
mkfs.fat -F 32 /dev/sda1

Set up the swap partition
mkswap /dev/sda2

Set up the root partition
mkfs.ext4 /dev/sda3


mount /dev/sda3 /mnt
swapon /dev/sda2


lsblk shows the new mounts
...
sda
-sda1 ....
-sda2 .... [SWAP]
-sda3 .... /mnt

Install linux
pacstrap /mnt base linux linux-firmware

If not able to install, check the iwctl settings above.

Now chroot into the base system in /mnt to complete more installation
arch-chroot /mnt

pacman -S networkmanager wpa_supplicant wireless_tools netctl 

Enable the network manager on boot
systemctl enable NetworkManger

Create a new initramfs by running the following command for normal Linux:
mkinitcpio -p linux

Set localization.  Uncomment the location you want (en_US.UTF-8 UTF-8) in
pacman -S nano
nano /etc/locale.gen
locale-gen

ln -sf /usr/share/zoneinfo/America/New_York /etc/localtime

Create a non-root user
useradd -m -g users -G wheel dan
passwd dan

Escalate to sudo privs
pacman -S sudo
EDITOR=nano visudo
Uncomment the line `%wheel ALL=(ALL:ALL) ALL` and save.

Install a boot manager
pacman -S grub efibootmgr dosfstools os-prober mtools
mkdir /boot/EFI
mount /dev/sda1 /boot/EFI

lsblk shows
sda
-sda1 .... /boot/EFI
-sda2 .... [SWAP]
-sda3 .... /

Install grub
grub-install -–target=x86_64-efi -–bootloader-id=grub_uefi -–recheck

To configure the locale of the grub boot screen run the following command
cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo

Generate the grub config
grub-mkconfig -o /boot/grub/grub.cfg

exit and reboot
remove the usb drive

Log in as user dan with password entered above.
