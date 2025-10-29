
For gnome

Remove mutter overlay key (you can set another key binding in the settings)
set to ''

gsettings list-recursively | grep '<Super>'


https://askubuntu.com/questions/1441271/how-do-you-disable-all-super-key-shortcuts-in-gnome


https://askubuntu.com/questions/1308834/remap-super-key


See: Input configurationhttps://github.com/swaywm/sway/wiki
https://man.archlinux.org/man/sway-input.5

input <identifier> xkb_file <file_name>

Sets all xkb configurations from a complete .xkb file. This file can be dumped from xkbcomp $DISPLAY keymap.xkb. This
setting overrides xkb_layout, xkb_model, xkb_options, xkb_rules, and xkb_variant settings.

Sway mouse options - there's a speed one too.

input <identifier> accel_profile adaptive|flat    Sets the pointer acceleration profile for the specified input device.



https://unix.stackexchange.com/questions/16039/is-it-possible-to-add-more-modifiers

Shift: Shift_L, Shift_R
Lock: Caps_Lock                        Map to hyper?
Control: Control_L, Control_R
Mod1: Num_Lock
Mod2: Meta_L, Meta_R                   Command
Mod3: Alt_L, Alt_R                Option
Mod4: Hyper_L, Hyper_R
Mod5: Super_L, Super_R

The distribution of Alt/Hyper/Meta/Super/NumLock amongst Mod1–5 is arbitrary



XKBhttps://wiki.archlinux.org/title/X_keyboard_extension

https://emacsnotes.wordpress.com/2022/10/30/use-xkb-to-setup-full-spectrum-of-modifiers-meta-alt-super-and-hyper-for-use-with-emacs/


input <device name> {
left_handed enabled
tap enabled
natural_scroll disabled
dwt enabled
accel_profile "flat" # disable mouse acceleration (enabled by default; to set it manually, use "adaptive" instead of "flat")
pointer_accel 0.5 # set mouse sensitivity (between -1 and 1)
}
Now

Add an Emoji, Sticker, or GIF
t
a

