# 1. Pre-installation
### 1. Connect to the internet
```
iwctl
station wlan0 scan
station wlan0 get-networks
station wlan0 connect iptime5G
exit
ping ping.archlinux.org
```

### 2. Partition the disks
```
fdisk -l
cfdisk /dev/nvme0n1
```
|Mount point|Partition|Partition type|Size|
|:---:|:---:|:---:|:---:|
|/boot|efi_system_partition|EFI system|1G|
|Swap|swap_partition|Linux swap|4G|
|/|root_partition|Linux root (x86-64)||

### 3. Format the partitions
```
mkfs.ext4 /dev/nvme0n1p3
mkswap /dev/nvme0n1p2
mkfs.fat -F 32 /dev/nvme0n1p1
```

### 4. Mount the file systems
```
mount /dev/nvme0n1p3 /mnt
mount --mkdir /dev/nvme0n1p1 /mnt/boot
swapon /dev/nvme0n1p2
```
# 2. Installation
### 5. Select the mirrors
```
reflector --country "South Korea" --age 12 --protocal https --sort rate
reflector --country "South Korea" --age 12 --protocal https --sort rate --save /etc/pacman.d/mirrorlist
```

### 6. Install essential packages
```
pacstrap -K /mnt base linux linux-firmware
```

### 7. Fstab
```
genfstab -U /mnt >> /mnt/etc/fstab
```

### 8. Chroot
```
arch-chroot /mnt
```

### 9. Time
```
ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
hwclock --systohc
```

### 10. Localization
```
pacman -S vim
vim /etc/locale.gen
# Uncomment en_US.UTF-8 UTF-8
locale-gen
```
```
vim /etc/locale.conf
# Add LANG=en_US.UTF-8
```

### 11. Network configuration
```
vim /etc/hostname
# Add yourhostname
```
```
pacman -S networkmanager
systemctl enable NetworkManager
```

### 12. Root password
```
passwd
```

### 13. Boot loader
#### 13.1. GRUB
```
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --removable
grub-mkconfig -o /boot/grub/grub.cfg
```
#### 13.2. systemd-boot
```
bootctl install
vim /boot/loader/loader.conf
# default  arch.conf
# timeout  4
# console-mode max
# editor   no
blkid
vim /boot/loader/entries/arch.conf
# title   Arch Linux
# linux   /vmlinuz-linux
# initrd  /initramfs-linux.img
# options root=UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx rw
vim /boot/loader/entries/arch-fallback.conf
# title   Arch Linux (fallback initramfs)
# linux   /vmlinuz-linux
# initrd  /initramfs-linux-fallback.img
# options root=UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx rw
```

### 14. Reboot
```
exit
umount -R /mnt
reboot
```
# 3. Post-installation
### 15. Add a new user
```
useradd -m -G wheel username
passwd username
```
```
pacman -S sudo
EDITOR=vim visudo
# Uncomment %wheel ALL=(ALL) ALL
```
