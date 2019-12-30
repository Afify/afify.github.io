---
title: install_openbsd
---

<article markdown="1">
### install OpenBSD
</article>

<article markdown="1">

#### encrypt root disk
```sh
sysctl hw.disknames
dmesg | egrep '^(cd|wd|sd|fd). at '
diskname=sd0
diskname_a=$diskname'a'
ram_diskname='r'$diskname'c'
```

dd if=/dev/urandom of=/dev/$ram_diskname bs=1m
fdisk -iy -g -b 960 $diskname
disklable -E $diskname
a a\n\n\n\n\nRAID\nw\nq\n
bioctl -c C -l $diskname_a softraid0

newdisk=$(dmesg | grep CRYPTO | awk '{print $1}')
newdisk_a=$newdisk'a'
ram_newdisk='r'$newdisk'c'

cd /dev && sh MAKEDEV $newdisk
dd if=/dev/zero of=/dev/$ram_newdisk bs=1m count=1
exit

# syspatch
echo 'https://ftp.openbsd.org/pub/OpenBSD
https://fastly.cdn.openbsd.org/pub/OpenBSD' > /etc/installurl
syspatch

# fstab
cp /etc/fstab /etc/fstab.bak
sed -i 's/datasize-cur=768M/datasize-cur=4096M/' /etc/login.conf
sed -i 's/datasize-max=768M/datasize-max=4096M/' /etc/login.conf

# apmd
rcctl enable apmd
#TODO -z options from supproted in dmesg
rcctl set apmd flags -A -z 7
rcctl start apmd

# doas
# TODO some commads no pass
echo 'permit hassan' > /etc/doas.conf

# disable inteldrm
config -fe /bsd
disable inteldrm*
quit
doas sha256 -h /var/db/kernel.SHA256 /bsd

reorder kernel

# change shell to bash
chsh -s /usr/local/bin/bash


###############################################################################


# AFTER INSTALLATION


###############################################################################


# install manual main programes

# mount data
mkdir /mnt/data
mount sd0i /mnt/data

# dwm
cd /mnt/data/dev/suckless/dwm-6.2/
vim config.mk
make clean install

# azan
cd ../azan
make clean install

# splanner
cd ../splanner
make clean install

# st
cd ../st*
vim config.mk
make clean install

# surf
cd ../st*
vim config.mk
make clean install


# sent
cd ../st*
vim config.mk
make clean install

# dmenu
cd ../st*
vim config.mk
make clean install

# slstatus
cd ../st*
vim config.mk
make clean install

# pull dotfiles

# vifm
sh .scripts/vifm_install.sh

# tcc
sh .scripts/tcc_install.sh

# nerdfonts

# install pkg_base
sh .scripts/pkg_base.sh

# config theme and font
sh .scripts/deploy_theme...

# config ssh and gpg
sh .scripts/deploy_backup...

</article>
