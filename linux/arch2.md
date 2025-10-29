
# Install Arch

Boot from USB
```
ip link
archinstall
```

Choose sway.


# Terminal text size

Terminal text config:
`~/.config/foot/foot.ini`
```
font=monospace:pixelsize=20
```


# Key binding setup

Qemu fails too slow - try this on the real machine.

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

Since the xkb config above didn't work.
or from: https://man.archlinux.org/man/xkeyboard-config-2.7.en#Caps_Lock_behavior
And https://github.com/swaywm/sway/issues/8443
```
set $mod Mod4
input type:keyboard {
    xkb_options caps:hyper
}
```

Run this in terminal for keymap debugging
`xkbcli interactive-x11` wayland fails?


# Key bindings 

Read `/etc/sway/config`

```
cp /etc/sway/config ~/.config/sway/config
```

mod shift c - reload sway config

Or from cli: `swaymsg reload`

mod b - split horizontally
mod v - split vertically

mod s - stacking
mod w - tabbed
mod e - toggle split

mod f - toggle fullscreen

mod enter - Terminal

mod shift q - Kill process

mod d - launcher

mod r - resize mode (then arrows, esc to exit)

mod shift space - toggle floating

https://github.com/swaywm/sway/wiki/Shortcut-handling



# rofi is best and has wayland support now

```
sudo pacman -S rofi
```

```
set $menu rofi -show combi -modes combi -combi-modes "window,drun,run"
```

Dark theme
https://github.com/davatorium/rofi/blob/next/themes/android_notification.rasi


# Sway yasm

https://github.com/pancsta/sway-yasm


# Mako - notifications 

```
sudo pacman -S inotify-tools mako

notify-send "foo"
```

Dismiss all notifications:
```
makoctl dismiss -a
```

Or make a sway keybinding:
```
bindsym $mod+period exec makoctl dismiss -a
```


# AUR - yay

AUR stores community packages. A package may just download a binary and install it in the right place.

`yay` is a package manager for AUR. Since it is also a AUR package you need to build it to bootstrap it. Afterwards
you can use it to manage it's own updates.

Build [yay](https://github.com/Jguer/yay)
```
sudo pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

# TODO pbcopy pbpaste alias

# Foot terminal

Add this to .bashrc
Then ctrl+shift+z will jump to last prompt.
```
PROMPT_COMMAND='printf "\e]133;A\e\\"'
```

# Chrome

Try setting: `chrome://flags/#smooth-scrolling`

Gemini says I need to enable Ozone for wayland.
```
Option 1: Use the Chrome flags page
Open Chrome and type chrome://flags into the address bar.
Search for "Preferred Ozone platform".
```

If screensharing is broken:
```
--enable-features=WebRTCPipeWireCapturer
```


Install from AUR
https://aur.archlinux.org/packages/google-chrome

```
yay -S google-chrome
```

Or manually from AUR
```
git clone https://aur.archlinux.org/google-chrome.git
cd google-chrome
makepkg -si
```

Or install it via flatpak
(potentially some issues due to nested sandboxing - perhaps use the AUR version?)
```
sudo pacman -S flatpak
flatpak install flathub com.google.Chrome
flatpak run com.google.Chrome
```

https://flathub.org/en/apps/com.google.Chrome


# Jetbrains

Install toolbox

https://www.jetbrains.com/help/toolbox-app/toolbox-app-silent-installation.html#tba_installation

```
yay -S jetbrains-toolbox
```

Install Goland, Rider etc via toolbox.


Make sure smooth scrolling is enabled.

Figure out key bindings for command key.



# Other things to install?

Thunar, Nautilus, Dolphin,
```
sudo pacman -S thunar
```

Nautilus has a lot of deps!

Dolphin has a flatpak. Seems good.

Note flatpak uses pulseaudio (no pipewire yet)

Flatpak:
Bitwig
Bitwarden?
Calculator
Resources
Clock



