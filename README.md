# DebiArch

## Install and use debian just like you would arch



1. Download a Debian live standard CD: https://mirror.us.leaseweb.net/debian-cd/current-live/amd64/iso-hybrid/debian-live-11.3.0-amd64-standard.iso
2. Login with `user:live` (if needed, it will probably automatically login)
3. Check that you have internet with `ip a`. If you're using ethernet it should already be connected, otherwise you'll need to configure  `interfaces(5)` and probably `wpa_supplicant(8)`

```
sudo su -
curl https://raw.githubusercontent.com/mai-gh/debiarch/main/pacman > /usr/bin/pacman
chmod +x /usr/bin/pacman
pacman -Sy
pacman -S openssh-server
passwd user
systemctl start sshd
echo "deb http://deb.debian.org/debian bullseye contrib" >> /etc/apt/sources.list
pacman -Sy
pacman -S arch-install-scripts debootstrap
fdisk /dev/vdX
mount /dev/vdX1 /mnt
debootstrap bullseye /mnt
cp /usr/bin/pacman /mnt/usr/bin/pacman
cp /etc/network/interfaces /mnt/etc/network/interfaces
genfstab /mnt > /mnt/etc/fstab
arch-chroot /mnt
export TERM=linux
mv -vn /usr/sbin/* /usr/bin/
rm -rf /usr/sbin
ln -sv /usr/bin /usr/sbin
ln -sfv /usr/share/zoneinfo/America/New_York /etc/localtime
echo HOSTNAME > /etc/hostname
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
locale-gen
pacman -S vim
pacman -Ss linux-image
pacman -S linux-image-5.10.0-10-amd64
pacman -S grub-pc
grub-install --target=i386-pc /dev/vdX
echo "set root='(hd0,msdos1)'" > /boot/grub/grub.cfg
echo "linux /boot/vmlinuz-5.10.0-10-amd64 root=/dev/vdX1 rw" >> /boot/grub/grub.cfg
echo "initrd /boot/initrd.img-5.10.0-10-amd64" >> /boot/grub/grub.cfg
echo "boot" >> /boot/grub/grub.cfg
cat /boot/grub/grub.cfg
useradd -m -g users -s /bin/bash USERNAME
passwd
passwd USERNAME
pacman -S openssh-server python3
ln -sv /usr/bin/python3 /usr/bin/python
systemctl enable ssh
exit
reboot
```

this project is based on:

 - https://gist.github.com/Tookmund/abfc4cbd803915264e0c
 - https://gitlab.com/trivoxel-utils/deb-pacman
