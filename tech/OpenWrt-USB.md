Router storage space is only around 10M. I want to mount a USB, then maybe in the future build a server inside.

Either way below succeeds. The conclusion is that I need to recompile the kerner or libc.


# Directly use "mount"

Check /dev

    root@OpenWrt:~# ls /dev/sd*
    /dev/sda   /dev/sda1

After make a directory in /mnt, use mount. But it doesn't work:

    root@OpenWrt:~# mount /dev/sda1 -t vfat /mnt/sda1
    mount: mounting /dev/sda1 on /mnt/sda1 failed: No such device
    root@OpenWrt:~# mount /dev/sda1  /mnt/sda1
    mount: mounting /dev/sda1 on /mnt/sda1 failed: Invalid argument

### Further trials
Maybe there is something wrong with usb drivers.
After `opkg update`, some packages cannot be installed because of the inconsistent of versions.

    root@OpenWrt:~# opkg install kmod-usb-uhci
    Installing kmod-usb-uhci (5.10.115-1) to root...
    Downloading http://downloads.lede-project.org/snapshots/targets/ramips/mt7621/packages/kmod-usb-uhci_5.10.115-1_mipsel_24kc.ipk
    Collected errors:
    * satisfy_dependencies_for: Cannot satisfy the following dependencies for kmod-usb-uhci:
    *      kernel (= 5.10.115-1-6aed52e40bdb1a3688736ee0c955e179) *
    * opkg_install_cmd: Cannot install package kmod-usb-uhci.

My openwrt system is 4.xx.xxxxx.  To Solve this problem, I need to re-compile the kernel.



Another idea is:

# Official tutorial
https://openwrt.org/docs/guide-user/storage/usb-drives

Check block information:
    root@OpenWrt:~# block info
    /dev/mtdblock5: UUID="ea97b996-5ffbeb6d-f107abe5-90754c55" VERSION="4.0" MOUNT="/rom" TYPE="squashfs"
    /dev/mtdblock6: MOUNT="/overlay" TYPE="jffs2"
    /dev/sda1: UUID="A6E9-703C" LABEL="NO NAME" VERSION="FAT32" TYPE="vfat"

Problem is that the command lsusb cannot work, so that the command gdisk(maybe depends on lsusb) cannot work either. Like:

    root@OpenWrt:~# lsusb -t
    Error relocating /usr/lib/libusb-1.0.so.0: __clock_gettime64: symbol not found
    Error relocating /usr/lib/libusb-1.0.so.0: __nanosleep_time64: symbol not found
    Error relocating /usr/lib/libusb-1.0.so.0: __pthread_cond_timedwait_time64: symbol not found
    Error relocating /usr/lib/libusb-1.0.so.0: __timerfd_settime64: symbol not found
    Error relocating /usr/lib/libudev.so.1: __lstat_time64: symbol not found

Here is the advice to upgrade libc:
https://forum.openwrt.org/t/openssl-util-1-1-1l-1-error-relocating-usr-lib-libssl-so-1-1-time64-symbol-not-found/108861


# other reference
after recompiling, consider https://blog.csdn.net/qq_43450413/article/details/107145575