# Installation of Arch Linux

##  1. Pre-Installation
### 1.1 Acquire an installation image 

For installation of arch linux you will need first an iso image of Arch.
You will get it here on the [download](https://archlinux.org/download/) page.

**Important**: Verify the image you downloaded for corruption. For that download the gpg signature as well.

```shell
gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig
```

### 1.2 Installation Medium
Use a USB flash drive or an optical disc.

Writing the image on the medium:

```shell
dd bs=4M if=path/to/archlinux-version-x86_64.iso of=/dev/disk/by-id/usb-My_flash_drive conv=fsync oflag=direct status=progress
```

### 1.3 Boot in the live evironment

Now you can boot into the live environment.
Be sure to explicitly boot into the USB/Optical Drive and disable *Secure Boot*, refer to the motherboard's manual.

**Note:**
If your device is beeping like crazy when using tab completion or using backspace, unload the pcspkr kernel module.
```shell
rmmod pcspkr
```

### 1.4 Verifying the boot mode

```shell
cat /sys/firmware/efi/fw_platform_size
```

If the command returns 64, then system is booted in UEFI mode and has a 64-bit x64 UEFI.
If the command returns 32, then system is booted in UEFI mode and has a 32-bit IA32 UEFI;
while this is supported, it will limit the boot loader choice to systemd-boot. 

If the file does not exist, the system may be booted in BIOS (or CSM) mode.
If the system did not boot in the mode you desired (UEFI vs BIOS), refer to your motherboard's manual.

### 1.5 Connection to the internet

- Ethernet - Just plug in the cable, the live installation should just do everything
- Wireless - Use **iwtctl** 
    ```shell
    device list
    device <device> set-property Powered on
    adapter <adapter> set-property Powered on
    station <device> scan
    station <device> get-networks
    station <device> connect <SSID>
    ```
- Mobile Broadband - Use **nmcli**

### 1.6 Partitioning the disks
First, find out what disk you want to use.

```shell
fdisk -l
```
Then after finding the disk partition it with:

```shell
fdisk /dev/disk_that_you_use
```
TODO Append here how to partition with fdisk

### 1.7 Formatting the partitions
To create an ext4 filesystem use mkfs.ext4.
```shell
mkfs.ext4 /dev/sys_partation
```
If you use a swap partition:
```shell
mkswap /dev/swap_partition
```
To format the EFI System Partition:
```shell
mkfs.fat -F 32 /dev/efi_partition
```

### 1.8 Mounting the partitions

To mount the system partition on /mnt
```shell
mount /dev/sys_partation /mnt
```

After that mount the EFI partition
```shell
mount --mkdir /dev/efi_partition /mnt/boot
```

After that activating the swap partition
```shell
swapon /dev/swap_partition
```

## 2. Installation

### 2.1 Mirror list
Maybe you want to update the mirror list with **reflector**.
```shell
reflector
```

### 2.2 Installation of essential packages
```shell
pacstrap -K /mnt base base-devel linux linux-firmware networkmanager grub efibootmgr neovim man-db
```

- base - Basic tools for a system.
- base-devel - Basic development tools.
- linux - Linux kernel, you could use linux-zen or linux-hardened as well. [List of options](https://wiki.archlinux.org/title/Kernel#Officially_supported_kernels)
- linux-firmware - Basic firmware
- networkmanager - NetworkManager daemon. You could use other [alternatives](https://wiki.archlinux.org/title/Network_configuration#Network_managers).
- grub - GRand Unified Bootloader. Bootloader that I use.
- efibootmgr - efibootmgr utility. I need that for grub and for managing my boot entries.
- neovim - Neovim text editor
- man-db - Man page database

## 3. Configure the system
### 3.1 fstab

For the system to mount the disks automatically on boot:
```shell
genfstab -U /mnt >> /mnt/etc/fstab
```
Check the result with `cat /mnt/etc/fstab`.

### 3.2 Chroot

Chroot in the system.
```shell
arch-chroot /mnt
```
### 3.3 Time

Setting the time zone:
```shell
ln -s /usr/share/zoneinfo/<REGION>/<CITY> /etc/localtime
```
Run hwclock to generate /etc/adjtime
```shell
hwclock --systohc
```

### 3.4 Localization

Edit the `/etc/locale.gen` and uncomment the locales you want to have.
Then run:
```shell
locale-gen
```

Create the `/etc/locale.conf` and set the LANG variable.
```
/etc/locale.conf
-------------------------
LANG=en_US.UTF-8
```

### 3.5 Hostname

Set the hostname in the `/etc/hostname` file
```
/etc/hostname
-------------------------
examplehostname
```

### 3.6 Root Password

Set the root password with passwd:
```shell
passwd
```

### 3.8 Installing the boot loader

Install the amd or intel microcode.

Only install the one you need
```
pacman -S amd-ucode # amd cpu
pacman -S intel-ucode # intel cpu
```

Installing grub to EFI partition:
```shell
grub-install --target=x86_64-efi --efi-directory=<efi_directory> --bootloader-id=grub
```

Generating config:
```shell
grub-mkconfig -o /boot/grub/grub.cfg
```

### 3.9 Reboot

Reboot and enjoy your hopefully successful installation
