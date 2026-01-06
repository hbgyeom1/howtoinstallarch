### 1. Connect to the internet
```
iwctl station wlan0 scan
iwctl station wlan0 get-networks
iwctl station wlan0 connect <SSID>
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
