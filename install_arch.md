---
title: install_arch
---

<article markdown="1">
### install Arch
</article>

<article markdown="1">
##### [1] Checking Internet connect
- wifi-menu
- ping 8.8.8.8
##### [2]  Update the system clock
- timedatectl set-ntp true
- systemctl start systemd-timesyncd.service
- hwclock --show
- hwclock --systohc # set hardware clock from system clock
##### [3]  Disk partition
- lsblk
- gdisk /dev/nvme0n1
- boot         +550M
- root         +25G
- swap         +2G
- efi          => EF00
- linux system => 8300
- swap         => 8200
- o            => clean
- n            => new
- w            => write
- mkfs.fat -F32 /dev/nvme0n1p1     # boot
- mkswap /dev/sda2            # swap
- swapon /dev/sda2            # swap
- mkfs.ext4 /dev/sda3         # root
- mkfs.ext4 /dev/sda4         # home
#####  [4]  Mount the file system
- mount /dev/sda4 /mnt            #root
- mkdir /mnt/home
- mkdir /mnt/boot
- mount /dev/sda3 /mnt/home       # home
##### [5]  Install base
- pacstrap /mnt base base-devel linux vim sudo dialog
 ##### [6]  Fstab
 - genfstab -U /mnt >> /mnt/etc/fstab
 - discard
 ##### [7] mount to new installation
 - arch-chroot /mnt
 ##### [8]  Time zone, language, time
 - ln -sf /usr/share/zoneinfo/Asia/Riyadh /etc/localtime
 - hwclock --systohc
 - echo en_US.UTF-8 UTF-8 > /etc/locale.gen
 - locale-gen
 - echo LANG=en_US.UTF-8 > /etc/locale.conf
 ##### [9] Create root password
 - passwd
 ##### [11]  Grub efi settings
 - mkdir /boot/efi
 - mount /dev/sda1 /boot/efi
 - pacman -S grub efibootmgr dosfstools mtools
 - grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=Arch --recheck
 - grub-mkconfig -o /boot/grub/grub.cfg
 - pacman -S linux
 ##### [12] Add user
 - useradd -m -g users -s bin/bash/ hassan
 - passwd hassan
 - visudo echo ' hassan ALL=(ALL) ALL' 
 ##### [13] Install main packages

 - sudo pacman -S - < ~/.man/install_pacman_main.txt
 - git clone https://github.com/Afify/install_arch.git
 - sh install.sh

# dwm
# st
# surf
# login
# xinput
# icon theme

 ##### [14] enable services
 - systemctl enable NetworkManager
 - systemctl enable lightdm
 - systemctl enable bluetooth
 ##### [15] Change the hostname
 - hostnamectl set-hostname alien
 ##### [16] Reboot
 - umount -R /mnt
 - reboot
 ------
 ### After login
 - sudo mkdir -p /mnt/data
 - sudo mount /dev/sda1 /mnt/data
 - sh /mnt/data/config/.scripts/deploy_dotfiles.sh
 - sh /mnt/data/config/.scripts/deploy_theme_icons_keyb.sh'
 - sh ~/.scripts/add_data_to_fstab.sh
 - sh ~/.scripts/install_yaourt.sh
 - sh ~/.scripts/install_aur_main.sh
 ---
 ##### [6] optional packages
 - virtualbox
 - screen recorder
 - obs-studio
 - hashcat
 - wireshark
 - steam
</article>
