+++
title = "Arch_ins_conf"
date = "2021-12-28T11:23:54+08:00"
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["", ""]
keywords = ["", ""]
description = "archlinux的安装配置向导"
showFullContent = false
readingTime = false
+++

## 01 Pre Ins

```shell
systemctl stop reflector.service

iwctl # station wlan0 connect[-hidden] SSID

timedatectl set-ntp true # timedatectl status

fdisk -l
fdisk /dev/DISK

mkfs.fat -F32 /dev/EFI
mkfs.ext4 /dev/ROOT

mount /dev/ROOT /mnt
mkdir /mnt/boot
mount /dev/EFI /mnt/boot
```

## 02 Ins base pkgs

```shell
## https://mirrors.cloud.tencent.com/archlinux/$repo/os/$arch
vim /etc/pacman.d/mirrorlist

pacstrap /mnt base base-devel linux linux-firmware fish neovim networkmanager grub efibootmgr intel-ucode
```

## 03 Conf the system

```shell
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt

ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc

nvim /etc/locale.gen # uncomment en_US.UTF-8 and zh_CN.UTF-8
locale-gen
nvim /etc/locale.conf # add LANG=en_US.UTF-8

nvim /etc/hostname # set hostname
nvim /etc/hosts
# 127.0.0.1 localhost
# ::1 localhost 
# 127.0.1.1 linux.localdomain linux
systemctl enable NetworkManager.service

passwd # root password
useradd -m -G wheel -s /usr/bin/fish username
passwd username # user password
EDITOR=nvim visudo # uncomment "%wheel ALL=(ALL) ALL"
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```

## 04 Reboot

```shell
exit
umount -R /mnt
reboot
```

## 05 Post_Ins

```shell
nmcli device wifi connect SSID password PASSWORD [hidden yes]

# https://mirrors.cloud.tencent.com/archlinuxcn/$arch
nvim /etc/pacman.conf # enable Colors and multilib, and archlinuxCN mirror
sudo pacman -Syyu
sudo pacman -Sy archlinuxcn-keyring
```

## WM and tools

```shell
xorg xorg-xinit xdg-user-dirs xclip lxsession
xf86-video-intel vulkan-intel
alsa-utils pulseaudio-alsa bluez bluez-utils pulseaudio-bluetooth
noto-fonts noto-fonts-cjk noto-fonts-emoji nerd-fonts-jetbrains-mono
fcitx5-im fcitx5-rime fcitx5-material-theme
lxappearance qt5ct kvantum
capitaine-cursors papirus-icon-theme material-gtk-theme kvantum-material-theme
bspwm sxhkd kitty rofi picom dunst
git openssh npm feh scrot lazygit fzf fd ripgrep ranger highlight tmux hugo udiskie unarchiver flameshot
chromium firefox code v2raya(v2ray) pomotroid-bin exa
```

## init conf

```shell
~/.xinitrc # exec bspwm
mkdir ~/.config/{bspwm,sxhkd}
cp /usr/share/doc/bspwm/examples/bspwmrc ~/.config/bspwm
cp /usr/share/doc/bspwm/examples/sxhkdrc ~/.config/sxhkd
# urxvt -> kitty
# dmenu_run -> "rofi -show run"

startx
```

## 06 Proxy

Start/Enable v2raya.service deamon. Activite `localhost:2017`, import url of subscription

## 07 Dotfiles

```shell
git clone --bare https://github.com/philopence/dotfiles.git $HOME/.dotfiles

alias dot '/usr/bin/git --git-dir=$HOME/.dotfiles --work-tree=$HOME'

dot config --local status.showUntrackedFiles no

dot checkout # remove files and dirs of conflict
```

## 08 Custom Keyboard

Create `/etc/udev/hwdb.d/99-custom-keyboard.hwdb` file, add this lines as follow.

```
evdev:input:b0003v*
 KEYBOARD_KEY_70039=leftctrl # bind capslock to leftctrl
 KEYBOARD_KEY_70035=capslock # bind grave to capslock
 KEYBOARD_KEY_700e0=grave    # bind leftctrl to grave
```

And then, run `systemd-hwdb update` and `udevadm trigger` command.

Or Install `interception-caps2esc` package, then edit `/etc/interception/udevmon.d/caps2esc.yaml` file, add this lines as follow:

```
- JOB: intercept -g $DEVNODE | caps2esc -m 2 | uinput -d $DEVNODE
  DEVICE:
    EVENTS:
      EV_KEY: [KEY_CAPSLOCK, KEY_ESC]
```

Start/Enable udevmon.service deamon.

## 09 Swapfile

switch to Root user

Note: count = 8192(8G)

```
dd if=/dev/zero of=/swapfile bs=1M count=8192 status=progress
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
nvim /etc/fstab
# /swapfile none swap defaults 0 0
```

## 10 Npm pkgs

```
prettier
live-server
@vue/cli

vscode-langservers-extracted
emmet-ls
vls
typescript
typescript-language-server
```
