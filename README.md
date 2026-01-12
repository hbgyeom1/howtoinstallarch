### Pre-installation
```
# 1. Connect to the internet
iwctl station wlan0 connect SSID
ping ping.archlinux.org

# 2. Update the system clock
timedatectl

# 3. Partition the disks
cfdisk /dev/the_disk_to_be_partitioned
```
|Mount point|Partition|Partition type|Size|
|:---:|:---:|:---:|:---:|
|/boot|efi_system_partition|EFI system|1G|
|Swap|swap_partition|Linux swap|4G|
|/|root_partition|Linux root (x86-64)||
```
# 4. Format the partitions
mkfs.ext4 /dev/root_partition
mkswap /dev/swap_partition
mkfs.fat -F 32 /dev/efi_system_partition

# 5. Mount the file systems
mount /dev/root_partition /mnt
mount --mkdir /dev/efi_system_partition /mnt/boot
swapon /dev/swap_partition
```
### Installation
```
# 6. Select the mirrors
reflector --country "South Korea" --age 12 --protocal https --sort rate --save /etc/pacman.d/mirrorlist

# 7. Install essential packages
pacstrap -K /mnt base linux linux-firmware

# 8. Fstab
genfstab -U /mnt >> /mnt/etc/fstab

# 9. Chroot
arch-chroot /mnt

# 10. Time
ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
hwclock --systohc

# 11. Localization
pacman -S vim
vim /etc/locale.gen # Uncomment en_US.UTF-8 UTF-8
locale-gen
vim /etc/locale.conf # Add LANG=en_US.UTF-8

# 12. Network configuration
vim /etc/hostname # Add yourhostname
pacman -S networkmanager
systemctl enable NetworkManager

# 13. Root password
passwd

# 14. Boot loader
## 14.1 GRUB
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --removable
grub-mkconfig -o /boot/grub/grub.cfg

# 15. Reboot
exit
umount -R /mnt
reboot
```
