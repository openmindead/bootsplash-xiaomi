# Description
Nothing special, yet another bootsplash theme for Manjaro Linux (Xiaomi logo). 

# Installation & configuration

`git clone https://github.com/openmindead/bootsplash-xiaomi.git`

`cd manjaro-bootsplash-mi`

Run `bootsplash-xiaomi.sh` to re-generate bootsplash-xiaomi STL model (optional)

Run `makepkg` to create Arch package and install it with `pacman -U %packagename%`

Append `bootsplash-xiaomi` hook in the end of `HOOKS` string of /etc/mkinitcpio.conf

Add `bootsplash.bootfile=bootsplash-themes/xiaomi/bootsplash` into `GRUB_CMDLINE_LINUX` string of `/etc/default/grub`

Run `sudo mkinitcpio -P && sudo update-grub` to update initial ram disk and grub configuration


# Some hints

The end result may vary on different configurations. For instance, it may be a good idea to add `i915` to `MODULES` section of `mkinitcpio.conf` in case of Intel-based system, or use modesetting kernel options, etc.
