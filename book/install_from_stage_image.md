# Venom Installation from staged image

Get venom's image [here](https://sourceforge.net/projects/venomlinux/)

## Preparing partition

Create your parition for venom installation.

```
# cfdisk
# mkswap /dev/sda1
# mkfs.ext4 -L Venom /dev/sda2
```
Create directory to mount created partition then mount it.

```
# mkdir /mnt/venom
# mount /dev/sda2 /mnt/venom
```

## Extract Venom's image

Extract Venom's image to mounted location.

```
# tar xvJpf venom-rootfs.txz -C /mnt/venom
```

## Enter chroot

Chroot into extracted venom's image.

```
# mount -v --bind /dev /mnt/venom/dev
# mount -vt devpts devpts /mnt/venom/dev/pts -o gid=5,mode=620
# mount -vt proc proc /mnt/venom/proc
# mount -vt sysfs sysfs /mnt/venom/sys
# mount -vt tmpfs tmpfs /mnt/venom/run
# mkdir -pv /mnt/venom/$(readlink /mnt/venom/dev/shm)
# cp -L /etc/resolv.conf /mnt/venom/etc/
# chroot /mnt/venom /bin/bash
```

## Configuring system

Configure hostname, timezone, clock, font, keymap and daemon:

```
# vim /etc/rc.conf
```

Configure /etc/fstab:

```
# vim /etc/fstab
```

Configure locales:

```
# vim /etc/locales
```
uncomment required locales then run:
```
# genlocales
```

Configure root password:
```
# passwd
```

Add user:
```
# useradd -m -G users,wheel,audio,video -s /bin/bash <your user>
```
then configure password for your user:
```
# passwd <your user>
```

Configure bootloader, grub:
```
# grub-install /dev/sdX
# grub-mkconfig -o /boot/grub/grub.cfg
```
> Note: replace 'X' with your partition drive

Exit chroot environment:
```
# exit
```

Unmount venom partition you mounted before:
```
# umount -v /mnt/venom/dev/pts
# umount -v /mnt/venom/dev
# umount -v /mnt/venom/run
# umount -v /mnt/venom/proc
# umount -v /mnt/venom/sys
# umount /mnt/venom
```

Then you can reboot now. You should have bootable Venom Linux installed into you drive
```
# reboot
```
