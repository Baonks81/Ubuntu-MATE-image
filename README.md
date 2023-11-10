# Ubuntu-MATE-image
Ubuntu MATE Building script for Nexus 7 grouper and others armhf

**If you just want to download Ubuntu MATE images for the Raspberry Pi the go to the Ubuntu MATE website:**

  * **https://ubuntu-mate.org/raspberry-pi/**

These "docs" are not comprehensive, but should be enough to get you started with
building your own Ubuntu based images for the Raspberry Pi.


## Building Images

  * Clone the Retro Home project
    * `git clone https://github.com/Baonks81/Ubuntu-MATE-image.git`

It is best to run the `Ubuntu-MATE-image` on an Ubuntu 22.04 x86 64-bit
workstation, ideally running in a VM via [Quickemu](https://github.com/quickemu-project/quickemu).
If using a fresh [Quickemu](https://github.com/quickemu-project/quickemu) VM
you'll need to set the `disk_size` parameter large enough to complete the build.
This can be achieved by adding `disk_size="64G"` to `ubuntu-mate-jammy.conf`
before running `quickemu` to create the VM. Alternatively you could mount
external storage into the container for the build area. You'll also need at
least to `sudo apt install git`.

## apt-cacher-ng

If you `apt-get install apt-cache-ng` the `Ubuntu-MATE-image` script will
automatically use the apt-cache-ng proxy to accelerate build performance.

## Build configuration

You can tweak some variables towards the bottom of the `Ubuntu-MATE-image` script.

```bash
FLAVOUR="Ubuntu-Mate"
IMG_QUALITY="-beta1"
IMG_VER="23.04"
IMG_RELEASE="mantic"
IMG_ARCH="armhf"
```

### Usage

The following incantation will build a Ubuntu MATE 22.04 armhf image for
Raspberry Pi.

```bash
sudo ./Ubuntu-MATE-image
```

The script will create `~/Build` in your home directory that will include the
caches for each stages and the image for putting on your Raspberry Pi SD card,
USB stick or SSD.

## Other Desktop

Modify the `stage_02_desktop` and `stage_03_snap` accordingly to adapt the script
for other Ubuntu flavours/desktops.

## Bootloaders

This is the bootloader configuration from Ubuntu 22.04.

### cloud-init

Remove these files:

  * `meta-data`
  * `network-config`
  * `user-data`
### README

```
An overview of the files on the /boot partition (the 1st partition
on the SD card) used by the Ubuntu boot process (roughly in order) is as
follows:

* bootcode.bin   - this is the second stage bootloader loaded by all pis with
                   the exception of the pi4 (where this is replaced by flash
                   memory)
* config.txt     - the configuration file read by the boot process
* start*.elf     - the third stage bootloader, which handles device-tree
                   modification and which loads...
* vmlinuz        - the Linux kernel
* cmdline.txt    - the Linux kernel command line
* initrd.img     - the initramfs
* meta-data      - meta-data for cloud-init; usually just contains the
                   instance id
* network-config - network configuration for cloud-init; edit this to set up
                   wifi access points and other networking settings
* user-data      - user-data for cloud-init; edit this to configure initial
                   users, SSH keys, packages, etc.
```
