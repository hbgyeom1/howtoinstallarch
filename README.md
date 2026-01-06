## 1. Connect to the internet
```
iwctl station wlan0 connect <SSID>
ping ping.archlinux.org
```

# 2. Partition the disks
```
cfdisk /dev/nvme0n1
```
|Mount point|Partition type|Size|
|:---:|:---:|:---:|
|/boot|EFI system|1G|
|Swap|Linux swap|4G|
|/|Linux root (x86-64)||

# 3. Format the partitions
```
mkfs.ext4 /dev/nvme0n1p3
mkswap /dev/nvme0n1p2
mkfs.fat -F 32 /dev/nvme0n1p1
```

# 4. Mount the file systems
```
mount /dev/nvme0n1p3 /mnt
mount --mkdir /dev/nvme0n1p1 /mnt/boot
swapon /dev/nvme0n1p2
```
