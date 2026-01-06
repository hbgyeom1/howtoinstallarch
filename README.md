# 1. Connect to the internet
```
iwctl station wlan0 connect <SSID>
ping ping.archlinux.org
```

# 2. Partition the disks
```
cfdisk /dev/nvme0n1
```
/boot
[swap]
/
