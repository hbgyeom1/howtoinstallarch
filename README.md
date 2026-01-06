# 1. Connect to the internet
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
|/|Linux root (x86-64)|Remainder|
