

https://tailscale.com/blog/tailscale-rustdesk-remote-desktop-access

https://www.ssp.sh/blog/macbook-to-arch-linux-omarchy/

# kinetic scroll


Kinetic scroll for firefox
apz.gtk.kinetic_scroll.enabled

https://github.com/jbouter/libinput-magic-mouse

https://daviewales.com/2022/11/01/almost-mac-like-touchpad-scrolling-in-firefox-on-ubuntu-22-04.html

https://github.com/bulletmark/libinput-gestures



Can configure ctrl keybindings to swap super and ctrl. But have to manually do it for each binding.


/usr/share/applications/google-chrome.desktop
Exec=/usr/bin/google-chrome-stable --disable-features=WaylandFractionalScaleV1 %U

On by default since v141?
--enable-features=UseOzonePlatform and --ozone-platform=wayland

--gtk-version=4


[text-bindings]
# \x03=Mod4+c  # Map Super+c -> Ctrl+c


https://codeberg.org/dnkl/foot/issues/325


There is a typo on these:
```
map ctrl+c       copy_to_clipboard
map ctrl+v        paste_from_clipboard
map super+question send_text all 7
map super+/ send_text all \037
map super+6 send_text all \036
map super+] send_text all \035
map super+backslash send_text all \034
map super+[ send_text all \033
map super+z send_text all \032
map super+y send_text all \031
map super+x send_text all \030
map super+w send_text all \027
map super+v send_text all \026
map super+u send_text all \025
map super+t send_text all \024
map super+s send_text all \023
map super+r send_text all \022
map super+q send_text all \021
map super+p send_text all \020
map super+o send_text all \017
map super+n send_text all \016
map super+m send_text all \015
map super+l send_text all \014
map super+k send_text all \013
map super+j send_text all \012
map super+i send_text all \011
map super+h send_text all \010
map super+g send_text all \007
map super+f send_text all \006
map super+e send_text all \005
map super+d send_text all \004
map super+c send_text all \003
map super+b send_text all \002
map super+a send_text all \001
map super+at send_text all \000
```

The control key generates the code 64 places below the code.



