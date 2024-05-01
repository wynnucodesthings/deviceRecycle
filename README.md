# deviceRecycle
### Hello and welcome to this guide written by wynnu aka moe on how to create a home server with an unused laptop.

Note: Make sure your device has at least a 64-bit processor and 2GB of RAM for a good experience!

## Before you start
For this guide you need to install an operating system called debian onto your machine.
### Prerequisite:
You need to burn an image of debian onto your USB drive using balenaEtcher:
https://etcher.balena.io/ <br/><br/>
The image of debian can be downloaded from:
https://www.debian.org/ <br/><br/>
Your internet modem must give you access to it's admin dashboard, have an ethernet port and allow you to change DHCP settings. <br/>

#### Note: following this tutorial WILL destroy all data that has been stored on this computer. please backup anything you want to keep.

## Step one: Preparing the installation
Assuming you have already burnt the image of debian onto your USB drive. Plug in the USB and turn on the laptop and boot into bios. <br/><br/>
Now once you're in bios. Head to boot and find the option for secure boot, follow by changing the secure boot option to other os. <br/>
#### Note: All devices have different bios firmwares and they tend to be different from one another. If you cannot find the option mentioned here, look on your own and find it in the bios. it's always there but labed differently.
Once you've changed your secure boot options, change the boot order so that the flash drive will boot instead of your default operating system, and boot through the USB,

## Step two: installation
If all goes well, you should boot into the usb. <br/><br/>
It should give you a promt asking you which of the installs you want to do, pick normal installation. <br/><br/>
Congrats! you are in linux now, but you're not done yet, you need to finish the install. Do everything as the install asks you(Like setting your user and language and country and such). Once you reach local device name, just set it to your hostname.local. for your network adapter, pick the one that your LAN goes through. <br/><br/>

### Partitioning
Once you reach the disk partitioning section, pick the option that it recommends, DO NOT turn on LVM as we will not need this.

### Finishing your install

At the end of your install you get a few options you can choose to download before fully installing the system. pick ssh server and finish the install. <br/><br/>

The device should restart at the end. remove the USB once it starts restarting.

## Step three: DHCP
Once the device restarts, you will boot into your machine. <br/><br/>
before doing anything, head over to your routers admin panel. It's usually 192.168.8.1 or something similar. Once you're into it's dashboard enter your username and password. The username and password are usually both admin, although sometimes it's also written behind your router itself too. <br/><br/>
Once you're into the router, find settings related to DHCP. Mine for instance was in Advanced > Router > DHCP. In the DHCP settings you will have to bind your devices name to a certain ip. which will make it a static ip that won't change. For example I set it to 192.168.8.77.

Once that's done, restart your machine and router.

### Extra: If you want the laptop to not turn off when you've closed the lid, follow these steps:
Use nano to edit the file /etc/systemd/logind.conf:
```
sudo nano /etc/systemd/logind.conf
```
```
/etc/systemd/logind.conf
------------------------
##... more lines ...##
#HandleHibernateKey=hibernate
HandleLidSwitch=ignore
#HandleLidSwitchExternalPower=suspend
##... more lines ...##
```
Save your changes by pressing Ctrl+X, then Y and Enter.

## Step four: SSH access
First on the device, go and login with the username and password. when you're in, do `su -` and enter your root password. you will realize that nothing seems to be typing, don't worry, that's just a security measure just keep typing your password there. Once you're in follow up with the command: `visudo`. <br/><br/>
That should open an editor with a lot of things in it. worry not! we only need to change one thing there. head down to where you see User privilege specification. under that you should see ROOT, press A once and then under Root type your user and then just write whats written infront of root, but for your user. once that's done, press ESC on your keyboard and press : and then write wq and press enter. it should exit out of that. press the power button on your laptop and restart it.
<br/><br/> <br/><br/>
Once you're in do not login again because you do not need to touch the laptop itself anymore. <br/><br/>
On your computer or phone(preferably a computer) on the same network. Open a command promt and enter this simple command :`ssh username@ip` for example in my case it's `ssh moh@192.168.8.77` <br/><br/>
It should ask you for your password. enter your password and you will be into the system without even being infront of it!
## Step five: installing and setting up applications needed
### SMB - Samba
Samba is an open source implementation of the SMB profile, a Microsoft standard for accessing files over a network. SMB provides a native experience on Windows, almost like a USB Hard Drive, and has good support on macOS, Linux, iOS and through 3rd party apps on Android.
#### Installation
On your server, install the SMB daemon with the following command:
`sudo apt install samba` <br/>
Once that’s completed, you need a directory to store the files you will be sharing on the network. You may choose to create a folder in the /media/ directory. We will make the /media/myfiles folder for this guide.
<br/><br/>
#### Setting samba up on your server
The Samba configuration must be edited to show the folder. Edit it with `sudo nano /etc/samba/smb.conf`. <br/><br/> <br/><br/>
By default, Samba will treat any attempts to log in with the wrong credentials as a guest user. This can cause issues on Windows if you accidentally connect with the wrong password, since your shares will not appear. Change the line:
```
map to guest = bad user
```
to
```
map to guest = never
```
and also add the newly created folder to your config too:
```
[myfiles]
  path = /media/myfiles
  writeable=yes
  public=no
```
- `myfiles` is your share name, and will be used when connecting over the network
- `path` is the folder shared from your server
- `writeable=yes` allows the creation and editing of files
- `public=no` hides the share if the user isn’t authenticated

Save your changes by pressing Ctrl+X, then Y and Enter.
<br/><br/>
Lastly, run `sudo smbpasswd -a youruser` and set a password for Samba. This will be the password you’ll use on client machines to connect to the network storage. To restart Samba and make sure the changes go through, `run sudo systemctl restart smbd`.
### Connecting to your remote drive
On your Windows PC, you can right-click This PC in the Explorer, and select Map network drive. <br/>
Input two backslashes followed by the IP of your server(eg: \\\192.168.8.77\myfiles), make sure it’s valid by clicking “Browse” and seeing if your files are in there as they should be.
## Final step: jellyfin
Jellyfin, as advertised is "the volunteer-built media solution that puts you in control of your media. Stream to any device from your own server, with no strings attached. Your media, your server, your way." <br/> <br/>
Install it by using:
```
curl https://repo.jellyfin.org/install-debuntu.sh | sudo bash
```
Once it's done, open a browser and enter the address: <your_ip>:8096, there you can setup jellyfin. you can setup your directories aswell. You may create directories and upload files through your SMB Share and then select those folders in the Jellyfin web interface.
<br/>
You may also install the jellyfin application onto your ios or android device and enter your jellyfin ip(<yourip>:8096) to connect, and followed by your login information to login.

## The end
With that you have fully build your own home server which can be utilized for a variety of uses. you can also add additional functions to your server, such as: custom dns servers, custom vpns and even local servers for games, websites and etc.
