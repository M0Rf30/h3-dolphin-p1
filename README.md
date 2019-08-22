# Compile u-boot
git clone git://git.denx.de/u-boot.git
cd u-boot
make CROSS_COMPILE=arm-none-eabi- orangepi_plus2e_defconfig -j4
make CROSS_COMPILE=arm-none-eabi- -j4

# SDcard (1)
dd if=/dev/zero of=/dev/mmcblk0 bs=1M count=8

# SDcard (2)
mkfs.ext4 -O ^metadata_csum,^64bit /dev/mmcblk0p1

mount /dev/mmcblk0p1 /mnt
wget http://archlinuxarm.org/os/ArchLinuxARM-armv7-latest.tar.gz
bsdtar -xpf ArchLinuxARM-armv7-latest.tar.gz -C /mnt


# SDcard (3)
sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/mmcblk0 bs=1024 seek=8
# Change the UUID within custom/boot.cmd and run 
mkimage -A arm -O linux -T script -C none -a 0 -e 0 -n "Orange Pi One boot script" -d custom/boot.cmd /mnt/boot/boot.scr
