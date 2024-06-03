## 1 Upgrade Firmware
This document assumes the use of either Linux or a Mac.

After booting the Omega2(+) and getting into the console, set up the wifi and connect to TCOffice24. It has a password with no special characters, which I think are a problem if present.

Use the USB serial link
```
screen /dev/ttyUSB0 115200
```
for example to open a console to the USB-connected Omega2(+)

Then on the console
```
cd /tmp
wget http://repo.onioniot.com.s3.amazonaws.com/omega2/images/<latest>
```
The latest omega2 image as of 20240519 is `omega2-v0.3.4-b258.bin`
The latest omega2+ image as of 20240519 is `omega2p-v0.3.4-b258.bin`

Then try
```
sysupgrade omega2-v0.3.4-b258.bin
```

When the upgrade is finished and the Omega is rebooted, open the console (if the console was open for the upgrade step, you can see the boot process). Hit enter and check that the right FW version is installed (should say `Ω-ware: 0.3.4 b258`)

###Note The WiFi setup appears broken. Must use the following file for `/etc/config/wireless`. Probably a good idea to make a copy of the original `wireless` file.

Then run via the console `wifisetup`.  Should see local network such as TCOffice24. Select and type password (L8serscann3r).

Note there could be a restriction on special characters in passwords...

## 2 Moving `/root` to the SD Card and Making a swap file

Okay, this will slow things down since the SD Card is not RAM... what to do about that?

Making a really big swap file allows us to store large objects in "RAM", which may be useful for buffering sensor data for example.

Follow this [guide](https://github.com/pjobson/onion_omega2p_experiments/blob/master/docs/setting_up_sdcard_for_root_and_swap.md).

When you get to the section on formating the Linux partion of the SD card, it may be necessary to first reboot the Omega2 before issuing `mkfs.ext4 /dev/mmcblk0p2`.

Everything worked as expected.

## Booting from External Storage (SD card)

The problem with the route above is that there is not enough room to install the tools necessary to develop on the Omega2(+). 

Instead, we need to shift the `overlay` and boot from external storage, in our case the SD card.

Follow this [guide](https://docs.onion.io/omega2-docs/boot-from-external-storage.html#boot-from-external-storage), but only after following section 2 above.



## 4 Cross-compile Environment

To set up a cross-compile environment, do the following:

1. Install XCode
2. Install Warp, a modern command line terminal. If you like, install oh-my-zsh as it's pretty useful
3. Open Warp and install the XCode command line tools using
```
xcode-select --install
```
4. Check that the compiler support is there by entering
```
gcc -v && llvm-gcc -v && clang -v
```
which should produce the following output
```
Apple clang version 15.0.0 (clang-1500.1.0.2.5)
Target: arm64-apple-darwin23.5.0
Thread model: posix
InstalledDir: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin
Apple clang version 15.0.0 (clang-1500.1.0.2.5)
Target: arm64-apple-darwin23.5.0
Thread model: posix
InstalledDir: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin
Apple clang version 15.0.0 (clang-1500.1.0.2.5)
Target: arm64-apple-darwin23.5.0
Thread model: posix
InstalledDir: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin
```
5. Install a package manager, either HomeBrew or MacPorts. I use [MacPorts](https://www.macports.org). Just follow the instructions.
6. Create a dedicated case-senstive filesystem for OpenWrt development. I prefer it to be under my `Development` folder, but it can be anywhere on your main disk.
```
cd ~
hdiutil create \-size 20g \-type SPARSE \-fs "Case-sensitive HFS+" \-volname OpenWrt OpenWrt.sparseimage
hdiutil attach OpenWrt.sparseimage
```
7. You can check using the Finder that the `OpenWrt` filesystem has been mounted.


## 5 Native Development

1. We will work in `/root`. Create a development directory for our work
```
cd /root
mkdir dev
cd dev
```
2. Install support for `git`;
```
opkg install git git-http ca-bundle
```
3. asda
