# 1. Connect to the internet
```
iwctl station wlan0 scan
iwctl station wlan0 get-networks
iwctl station wlan0 connect *SSID*
```
```
ping ping.archlinux.org
```
# 2. Update the system clock
```
timedatectl
```

# 3. Partition the disks
```
fdisk -l
```
