enigmabox-openwrt
=================

OpenWrt package feed for the Enigmabox software suite.

How to build that stuff:

    $ git clone git://git.openwrt.org/openwrt.git
    $ cd openwrt

    $ vi feeds.conf

Your feeds.conf should look like this:

    src-git packages https://github.com/openwrt/packages.git
    src-git oldpackages http://git.openwrt.org/packages.git
    src-git management https://github.com/openwrt-management/packages.git
    src-git routing https://github.com/openwrt-routing/packages.git
    src-git telephony https://github.com/openwrt/telephony.git

    src-git enigmabox https://github.com/enigmagroup/enigmabox-openwrt.git

Next use that package system to incorporate the Enigmabox software suite:

    $ ./scripts/feeds update -a
    $ ./scripts/feeds install -a
    $ make defconfig
    $ make prereq

Then configure for your system:

    $ make menuconfig

## Building firmware for the Banana Pi

"Target System": Allwinner A1x/A20/A3x

"Target Profile": Bananapi

"Target Images": configure as you please. Example:
* Root filesystem images:
  * ext4: yes
    * Maximum number of inodes in root filesystem: 200000
    * Create a journaling filesystem: yes
  * GZip images: no
* Image Options:
  * Root filesystem partition size (in MB): 3600
  * Include kernel in root filesystem: yes
  * Include DTB in root filesystem: yes

Recently, OpenWrt decided to ~~fuck everything up~~ switch to musl by default.

Stick to uClibc manually:

"Advanced configuration options":
  * Toolchain Options
    * C Library implementation
      * Use uClibc

"Enigmabox":
* cfengine-promises: yes
  * Network profile: Rasperry Pi
* provision-grandstream: yes
* roundcube: yes
* teletext: yes
* webinterface: yes

The "webinterface" is the most important part, since it's selecting all required dependencies.

Quit and save your .config

Then:

    $ make

After about 30mins (depending on your machine), your image is ready:

    bin/sunxi/openwrt-sunxi-Bananapi-sdcard-vfat-ext4.img

