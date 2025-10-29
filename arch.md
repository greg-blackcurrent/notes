


Wayland

Sway WM

Thunar file manager


Try map capslock to something that sway can use as the modifier.

Menu key? Can I use this in jetbrains?

Map double shift to capslock.

Run this in terminal for keymap debugging
`xkbcli interactive-wayland`

mod shift c - for reload sway config

Or from cli: `swaymsg reload`


xkbcomp?

https://www.beatworm.co.uk/blog/keyboards/gnome-wayland-xkb



esc

shift l/r
control
option l/r
command l/r

capslock

fn not possible to map - as part of kb firmware.


ctrl   (control)
alt    (option)
meta   (command)
super  
hyper


https://wiki.archlinux.org/title/Sway#Change_modifier_to_CapsLock_and_keep_Super

Change modifier to CapsLock and keep Super
To change the modifier key to CapsLock and keep the Super key functional on a US keyboard layout, create ~/.config/xkb/symbols/custom with contents

~/.config/xkb/symbols/custom
```
xkb_symbols "basic" {
  include "us"
  name[Group1]= "English (US Custom)";
  key <CAPS> { [ Hyper_L ] };
  modifier_map Mod4 { Hyper_L };
  key <LWIN> { [ Super_L ] };
  modifier_map Mod5 { Super_L };
};
```
For other languages, edit the second and third lines. Then include this keyboard layout in your sway config near the top of the file:

~/.config/sway/config
```
input * xkb_layout custom
set $mod Mod4
set $super Mod5
```


// alts are ctrls, winkeys are metas, ctrls are supers  
partial modifier_keys  
xkb_symbols "cms_modkeys" {  
replace key <LALT> { [ Control_L, Control_L ] };  
replace key <LWIN> { [ Alt_L, Meta_L ] };  
replace key <LCTL> { [ Super_L ] };  
replace key <RALT> { [ Control_R, Control_R ] };  
replace key <MENU> { [ Alt_R, Meta_R ] };  
replace key <RCTL> { [ Super_R ] };  
}; // end


```
google-chrome-stable --enable-features=UseOzonePlatform --ozone-platform=wayland
```

```
https://blog.jetbrains.com/platform/2024/07/wayland-support-preview-in-2024-2/
```

https://www.reddit.com/r/swaywm/comments/134jtuj/macoslike_keybindings_super_in_sway/



# Sway
input "the_keyboard" {
# xkb_file: Obtained with `setxkbmap -display $DISPLAY -model chromebook -layout gb -option "shift:both_shiftlock" -print`
# Get the full original file with: `xkbcomp $DISPLAY keymap.xkb`
xkb_file /home/<user>/.config/<me>/keyboards/chromebook-keymab.xkb
}


Terminal text config:
`~/.config/foot/foot.ini`
```
font=monospace:pixelsize=20
```


```
ip link
archinstall
```

Additional packages takes ages.


Etch ISO to USB and boot from it.

Connect to network. Probably just `ip link` with DHCP ethernet.

## Partition the disk

```
fdisk -l /dev/sda (or likely nvme0)

n # new partition

p (default) # primary (probably doesn't matter since only two partitions, don't need extended, limitation of master boot record)

1 (default) # first partition

2048 (default) # start of first sector

+1G # last sector


t # change partition type

1 # partition 1

ef # EFI system partition


n # new partition

p (default) # primary

2 (default) # partition number

2099200 (default) # start of first sector

<big number> (default) # last sector


w # write changes
```

## Format the partitions

```
mkfs.ext4 /dev/sda2
mkfs.fat -F 32 /dev/sda1
```

## Mount the file systems

```
mount /dev/sda2 /mnt
mount --mkdir /dev/sda1 /mnt/boot
```

## Check the mirrors

Should I add 2degrees? Sydney was at the top.
`/etc/pacman.d/mirrorlist`

## Download the base system

```
pacstrap -K /mnt base linux linux-firmware
```

## Configure the system

```
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
ln -sf /usr/share/zoneinfo/Pacific/Auckland /etc/localtime
hwclock --systohc
local-gen
```

```
echo -e "LANG=en_UK.UTF-8\n" > /etc/locale.conf
echo -e "nux\n" > /etc/hostname
passwd
```


# Other

https://aswinmohan.me/better-fonts-on-linux

