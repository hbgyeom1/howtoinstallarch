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

### 16. Add a graphical user interface
```
sudo pacman -S xorg-server i3-wm sddm wezterm gvim
```
