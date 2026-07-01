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
    8.1) `ln -sf /usr/share/zoneinfo/... /etc/localtime`, `hwclock --systohc`
    8.2) Uncomment `en_US.UTF-8` line in file `/etc/locale.gen`, then execute `locale-gen`,
        and `echo LANG=en_US.UTF-8 > /etc/locale.conf`
    8.3) Change hostname, root password and user account
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

