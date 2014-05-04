OW2 Project
===

A step by step guide for building an OpenWRT firmware, installing and configuring it for TP-LINK MR3020 or MR3040 devices
---

Our objective is to obtain fully configured small device based on TP-LINK MR3020 or MR3040 router hardware with OpenWRT as firmware. It can be used for:
* Compact router with 3G/4G modem support
* Video surveillance (by USB web cam) device with ethernet, wifi or 3G/4G connectivity
* Remote, internet control of lighting, heating, automating etc. (need hardware hacks and additional components)
* Remote measure of temperature, air humidity, pressure and/or some other sensor reading (again need hw works)
* A lot of other things that can be powered by 400 MHz MCU and linux inside

### Below are steps to build and configure OpenWRT:

1. Download the latest OpenWRT repo snapshot and all feeds by following commands:

`git clone git://git.openwrt.org/openwrt.git`

`cd openwrt`

`./scripts/feeds update -a`

`./scripts/feeds install -a`

2. Perform `make menuconfig` and go to "Target Profile" in the main menu. Select "TP-LINK TL-MR3020" or "-3040", according to your hardware. Exit by pressing ESC ESC two time and confirm saving the config.
3. Perform `make defconfig` (that creates general configuration of the build system including a check of dependencies and prerequisites for your selected system
4. Now you can already build your system with default settings/environment (it can take from 1 to 3 hours depend on your host performance, internet connection speed and other factors...

`time make`
5. `make menuconfig` and select:
- Base System -> block-mount (\*)
- Base System -> Boot Loaders -> uboot ( )   *<--  remove selection*
- Kernel Modules -> Filesystems -> kmod-fs-ext4 (\*)
- Kernel Modules -> USB Support -> kmod-usb-storage-extras (\*), kmod-usb-uhci (\*)

Also you can select a lot of additional items depend of what you need for your exact system. Below are some examples:
- Kernel Modules -> I2C support -> (M)   *<--  if you need i2c support - put (M) on required items*
- Kernel Modules -> W1 support ->  (M)   *<--  if you need 1-wire support - put (M) on required items*
- Multimedia -> mjpg-streamer (M)
- Multimedia -> motion (M)
- Network -> File Transfer -> curl (M)
- Network -> File Transfer -> wget (M)
- Network -> IP Addresses and Names -> ddns-scripts (M)
- Network -> Web Servers/Proxies  -> lighttpd (M) -> lighttpd-mod-\* (M)  *<--  put (M) on all lighttpd modules that you need*
- Network -> wpad (\*) *if you need full IEEE 802.1x Authenticator/Supplicant for enterprise Wi-Fi networks, otherwise select*   wpad-mini (\*)
- Utilities -> filemanager -> mc (M)
- Utilities -> digitemp (M)
- Utilities -> haserl (M)
- Utilities -> oww (M)
6. `time make V=s > build.log 2>errors.log`  *it will create also build log and errors log in corresponding files*

