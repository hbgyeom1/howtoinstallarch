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
|/boot|EFI system|1 GiB|
|[swap]|Linux swap|4 GiB|
|/|Linux x86-64 root (/)|Remainder|
