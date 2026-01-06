### 1. Connect to the internet
```
iwctl
station wlan0 scan
station wlan0 get-networks
station wlan0 connect <SSID>
exit

ping ping.archlinux.org
```

### 2. Partition the disks
```
fdisk -l
cfdisk /dev/the_disk_to_be_partitioned
```
|Mount point|Partition|Partition type|Size|
|:---:|:---:|:---:|:---:|
|/boot|efi_system_partition|EFI system|1G|
|Swap|swap_partition|Linux swap|4G|
|/|root_partition|Linux root (x86-64)||

### 3. Format the partitions
```
mkfs.ext4 /dev/root_partition
mkswap /dev/swap_partition
mkfs.fat -F 32 /dev/efi_system_partition
```

### 4. Mount the file systems
```
mount /dev/root_partition /mnt
mount --mkdir /dev/efi_system_partition /mnt/boot
swapon /dev/swap_partition
```

### 5. Select the mirrors

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
