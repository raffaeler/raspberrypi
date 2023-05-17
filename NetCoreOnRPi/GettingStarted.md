# Start developing with .NET Core and the Raspberry PI
## Preparing the Raspberry Pi for NetCore without installing the SDK or the Runtime

### Instructions of the Raspberry Pi3

*You can skip this if you already know how to setup the RPi*

There are several Linux distributions available for the RPi.
The most popular is the "Raspbian" a version derived from the Debian.
In this guide I used the "Raspbian Stretch with Desktop" because it is easier to administer locally. The setup works also for the "Raspbian Stretch Lite".

1. [Download the latest available Raspbian image](https://www.raspberrypi.org/downloads/raspbian/). Ensure you are downloading a "Stretch" release which has been released in August 2017 or later. This is important because .NET Core relies on the versions of the libraries that has been updated / patched in Stretch.

2. Extract the ZIP file. The file with the IMG extension must be flashed on a Micro-SD card. It is generally better high speed SDHC cards. In my demos I use the SanDisk Ultra 32GB Class 10 providing about 80MB/sec.

3. Flash the Image on the SD card. I use the popular utility "Win32DiskImager". Verify carefully to flash the card and not another card. And then verify again because the flash operation overwrite the content completely.

It can be useful to use a small 7" screen with an HDMI input. In my experience the HDMI input is easier to setup in comparison to the display interface on the flat cable.
These small monitors cost around 60$ for the 1024x600 resolution.
If you are using a standard computer HDMI 1080p monitor, it works and you don't need to modify the config.txt as for the following step.

4. If you have one **small** monitor, it may need to be configured manually via "config.txt" with the settings provided by the screen manufacturer. Standard HDMI 1080p monitors don't need this change. 
  After flashing the SDCard, close the Win32DiskImager, remove the card and re-insert it in the reader. This way you should be able to see and edit the config.txt from Windows.
  These are the settings I added at the very bottom of the config.txt file. Please check with your manufacturer's guide.
  If the next boot does not succeed, you can modify the config.txt again within Windows.

```
max_usb_current=1
hdmi_group=2
hdmi_mode=1
hdmi_mode=87
hdmi_cvt 1024 600 60 6 0 0 0
```

5. Put the SD Card in the Raspberry PI 3, ensure the peripherals (like the HDMI screen, an USB keyboard and USB mouse are plugged in), and finally boot by powering the RPi.
  Don't forget to plug an ethernet cable with the Internet connection. This is required to make software updates and install some mandatory packages. You can use wifi if you will. The visual configuration of the wifi from the Raspbian desktop  is simple (upper right portion of the desktop).

6. The Raspbian distribution will resize the main partition to fit the size of the SD Card automatically. Follow the instructions you will see on the screen.

7. Run the raspberry configurator to enable peripherals and remote access via SSH

**Important note**
The default username of this image is **pi** and the the default password is **raspberry**.
Since you are going to enable remote connections via SSH, it is super-important to change the password using this command:
```
passwd
```
More info [here](https://www.raspberrypi.org/documentation/linux/usage/users.md)

8. Enable SSHD and peripherals. The SSH connectivity is very useful to deploy your applications to the raspberry. Furthermore, the peripherals (such as the SPI, I2C, etc.) must be enabled before they can be used via software. This utility can be used to update the firmware too. It is generally better to save the SD content before updating the firmware. Since you just started installing, you can skip the backup.
  After this step you will need to reboot.
  The application used to configure the Raspberry PI, enable SSHD and the peripherals is the following
```
sudo raspi-config
```
*sudo is the way to elevate the privileges to root (admin) under Linux*

9. Install a SSH client on Windows. There are many, [Bitvise SSH client](https://www.bitvise.com/ssh-client) is my choice. It is a free client that provides both a terminal and the ability to transfer files.

10. Once the RPi rebooted, grab the IP address by opening a terminal and running this command:
```
ifconfig
```
you should see an output like this:
```
aabbccddee: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.50.100  netmask 255.0.0.0  broadcast 10.255.255.255
        inet6 fe80::8b6:909f:8c62:5cfa  prefixlen 64  scopeid 0x20<link>
        ether a4:22:aa:22:bb:11  txqueuelen 1000  (Ethernet)
        RX packets 1593388  bytes 1413758445 (1.3 GiB)
        RX errors 0  dropped 3187  overruns 0  frame 0
        TX packets 572897  bytes 269052134 (256.5 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
In this case the IP address is 10.0.50.100
If your network is DNS capable, you may want to prefer the device name instead which you can obtain with this command:
```
hostname
```

11. Making updates. This is the perfect moment to update the RPi. Here there are the commands used to make a complete update:
```
apt-get update	
apt-get upgrade	
apt-get dist-upgrade	
```
Verify whether something is wrong with your current packages:
```
dpkg -C
apt-mark showhold
```
And a final reboot:
```
reboot now	
```

This was the very basic steps to prepare the Raspberry for any usage. We can now go on with the [NetCorePackages](NetCorePackages.md).