##Upgrade Firmware

After booting the Omega and getting into the console, set up the wifi and connect to TCOffice24. It has a password with no special characters, which I think are a problem if present.

Use the USB serial link
```
screen /dev/ttyUSB0 115200
```
for example to open a console to the USB-connected Omega2(+)

Then on the console
```
cd /tmp
wget http://repo.onion.io.s3.amazonaws.com/omega/images/<latest>
```
The latest image as of 20240519 is `omega-v0.1.4-b336.bin`

There are also images for the Omega at `http://repo.onion.io.s3.amazonaws.com/omega2/images/<latest>` where `omega-v0.1.5-b128.bin` appears to be the latest. However, the 	`sysupgrade` image check fails for v0.1.5...

Then try
```
sysupgrade omega-v0.1.4-b336.bin
```

When the upgrade is finished and the Omega is rebooted, open the console (if the console was open for the upgrade step, you can see the boot process). Hit enter and check that the right FW version is installed (should say `Ω-ware: 0.1.4 b336`)
Then run via the console `wifisetup`.  Should see local network such as TCOffice24. Select and type password (L8serscann3r).

Note there could be a restriction on special characters in passwords...

