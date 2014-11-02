

create disk ==

use fdisk to create two partitions
fdisk /dev/sda
create new gpt partition table
create partition 1, size +256M (n, size +256M)
create partition 2, the rest (n, use defaults)
change partition 2 to type lvm (t, 23)
write and exit

create physical volume
pvcreate /dev/sda2

create volume group
vgcreate arch /dev/sda2

create logical volumes
swap 4GB
lvcreate -L 4G arch -n swap
root 8GB
lvcreate -L 4G arch -n root
home the rest
lvcreate -l +100%FREE arch -n swap

create file systems
mkswap -L swap /dev/mapper/arch-swap

mkfs.ext4 -L boot /dev/sda1
mkfs.ext4 -L root /dev/mapper/arch-root
mkfs.ext4 -L home /dev/mapper/arch-home

swapon /dev/mapper/arch-swap
mount /dev/mapper/arch-root /mnt
mkdir /mnt/{boot,home}

mount /dev/sda1 /mnt/boot
mount /dev/mapper/arch-home /mnt/home


