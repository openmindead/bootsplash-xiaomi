# Description
Yet another bootsplash theme for Manjaro Linux (Xiaomi logo). 

# Installation & configuration

`git clone https://github.com/openmindead/bootsplash-xiaomi.git`

`cd bootsplash-xiaomi`

Run `chmod +x bootsplash-packer bootsplash-xiaomi.sh`

Add `bootsplash.bootfile=bootsplash-themes/xiaomi/bootsplash` into `GRUB_CMDLINE_LINUX` string of `/etc/default/grub`

Append `bootsplash-xiaomi` hook in the end of `HOOKS` string of /etc/mkinitcpio.conf

Run `makepkg` to create Arch package and install it with `pacman -U %packagename%`

Run `sudo update-grub` to update initial ram disk and grub configuration


# Some hints

The end result may vary on different configurations. For instance, it may be a good idea to add `intel_agp i915` to `MODULES` section of `mkinitcpio.conf` in case of Intel-based system and use modesetting kernel options, etc. The best result is achieved with the use of direct kernel booting (w/o any bootloader, using just linux efistub). A similar command will do the trick:

`# efibootmgr -c -d /dev/sda -p 1 -L "Manjaro Linux" -l /vmlinuz-5.0-x86_64 -u 'ro initrd=\intel-ucode.img initrd=\initramfs-5.0-x86_64.img quiet udev.log_priority=3 <other options like cryptdevice, root, resume etc>' -v`
