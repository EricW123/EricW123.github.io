# My Notes

## Installing Linux OS

0. Prepare an USB with Linux ISO using Rufus or other tool.
1. Boot into UEFI or BIOS.
2. Reboot with the USB.
3. Now Linux will copy itself into RAM, forming a temporary Linux environment called `Live USB`.
4. Format and prepare target disk partition.
    4.1) Format: `mkfs.ext4 /dev/...`
    4.2) Mount partitions: `mkdir /mnt && mount ... /mnt`,
        `mkdir -p /mnt/boot/efi && mount ...(EFI part) /mnt/boot/efi`
5. install required packages
```bash
    pacstrap -K /mnt \
        base linux linux-firmware base-devel \
        networkmanager grub efibootmgr os-prober \
        vim sudo ...
```
6. `genfstab -U /mnt > /mnt/etc/fstab`
7. Enter this new system: `arch-chroot /mnt`
8. Basic configures:
    - `ln -sf /usr/share/zoneinfo/... /etc/localtime`, `hwclock --systohc`
    - Uncomment `en_US.UTF-8` line in file `/etc/locale.gen`, then execute `locale-gen`,
        and `echo LANG=en_US.UTF-8 > /etc/locale.conf`
    - Change hostname, root password and user account
        `echo <pcname> > /etc/hostname`, `passwd`, `useradd ...`, `passwd <username>`, `chgrp ...`
9. Install GRUB:
    ```bash
    grub-install \
        --target=x86_64-efi \
        --efi-directory=/boot/efi \
        --bootloader-id=GRUB
    grub-mkconfig -o /boot/grub/grub.cfg
    ```
10. Setup new system: `systemctl enable NetworkManager`
11. Exit Live USB and reboot into normal Linux system.

## Commonly used commands

* Pacman related
    - List explicitly installed packages: `sudo pacman -Qqe`

* Filesystem related
    - `lsblk -f`
    - `blkid`
    - `fdisk -l`
    - examine ext4 FS: `e2fsck`
    - format partition: `mkfs.ext4`, `mkfs.ntfs`, etc

* Network related
    - NetworkManager is a service, so need `systemctl enable NetworkManager` to start.
    - `nmtui`

* Permission
    - Display permission `ls -ld /path`, `stat -c "%a %n" /path`, `getfacl /path`, `namei -m /path`

* Curl
    - param `-L` means to follow redirect
    - `-O` means to keep filename, or `-o <filename>` for custom filename

## Steam

* Delete steam runtime files when need to install steam `rm -rf ~/.steam`
* Technically `steam (not AUR)` can run on xwayland (`xorg-xwayland`) with no other config required
* WallpaperEngine may not be runnable, try `waywallen`
* Share game file between win and linux:
    - install one win-only game in local, then install Proton by starting the game, then shift to games on other disk.
    - setting partition FSTYPE to `ntfs` in `/etc/fstab` will make it read-only after booting, set to `ntfs3` solves.
    - sometimes NTFS will meet issue that can be solved by switching to windows and run `chkdsk /f X:`

## Noctalia v5

* `noctalia msg panel-toggle ...` i.e. `launcher`

