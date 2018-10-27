# manjaro-bootsplash-mi
Nothing special, yet another bootsplash theme for Manjaro Linux (Xiaomi logo). 

# installation & configuration

`git clone https://github.com/openmindead/manjaro-bootsplash-mi.git`

`cd manjaro-bootsplash-mi`

run `bootsplash-xiaomi.sh` to generate bootsplash-xiaomi STL model

run `makepkg` to create Arch package and install it with `pacman -U %packagename%`

append `bootsplash-xiaomi` hook in the end of `HOOKS` string of /etc/mkinitcpio.conf

add `quiet bootsplash.bootfile=bootsplash-themes/xiaomi/bootsplash` into `GRUB_CMDLINE_LINUX` string of `/etc/default/grub`

run `sudo mkinitcpio -P && sudo update-grub` to update initial ram disk and grub configuration
