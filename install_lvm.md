Installing Archlinux 2014.10.01 on LVM
======================================

Installing Arch Linux: Prepare the system
=========================================

Setup the disks
---------------
Use fdisk to create two partitions
```
fdisk /dev/sda
```
create new gpt partition table
create partition 1, size +256M (n, size +256M)
create partition 2, the rest (n, use defaults)
change partition 2 to type lvm (t, 23)
write and exit

create physical volume
----------------------
```
pvcreate /dev/sda2
```

create volume group
```
vgcreate arch /dev/sda2
```

create logical volumes
----------------------
swap 4GB, root 8GB and the rest for home
```
lvcreate -L 4G arch -n swap
lvcreate -L 4G arch -n root
lvcreate -l +100%FREE arch -n swap
```

Create file systems
-------------------
Format the created partitions
```
mkswap -L swap /dev/mapper/arch-swap
mkfs.ext4 -L boot /dev/sda1
mkfs.ext4 -L root /dev/mapper/arch-root
mkfs.ext4 -L home /dev/mapper/arch-home
```

Mount the filesystems
---------------------
Mount the formatted filesystems
```
swapon /dev/mapper/arch-swap
mount /dev/mapper/arch-root /mnt
```
Create mountpoints in the new root disk and mount home and boot in it.
```
mkdir /mnt/{boot,home}
mount /dev/sda1 /mnt/boot
mount /dev/mapper/arch-home /mnt/home
```

