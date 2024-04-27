
# deviceRecycle

### Welcome to the deviceRecycle guide by wynnu (aka moe) on creating a home server from an unused laptop.

**Note:** Ensure your device has at least a 64-bit processor and 2GB of RAM for optimal performance.

## Before You Begin

Ensure you have installed the Debian operating system on your machine. You can download the Debian image from [here](https://github.com/wynnucodesthings/deviceRecycle/network/dependencies) and learn more about Debian at [debian.org](https://www.debian.org/).

Ensure your internet router allows access to its admin dashboard, has a LAN port, and enables DHCP settings.

**Note:** Following this tutorial will erase all data on your computer. Back up anything you want to keep.

## Step One: Preparing the Installation

1. Burn the Debian image onto a USB drive using [balenaEtcher](https://etcher.balena.io/).
2. Plug in the USB drive, turn on the laptop, and access the BIOS.

<!-- ![BIOS](path_to_your_image) -->

3. Navigate to the boot menu and set the boot order to prioritize the USB drive.

## Step Two: Installation

If successful, you'll boot into the USB drive. Follow these steps:

1. Select the normal installation option.
2. Follow the installation prompts, including setting user and language preferences.
3. When prompted for the local device name, set it to your hostname.local.
4. Choose the appropriate network adapter for your LAN.

<!-- ![Installation](path_to_your_image) -->

### Partitioning

Select the recommended disk partitioning option; do not enable LVM.

<!-- ![Partitioning](path_to_your_image) -->

### Completing the Install

Select additional options such as SSH server before completing the installation. Remove the USB drive when the device restarts.

<!-- ![Completing Install](path_to_your_image) -->

## Step Three: DHCP Configuration

1. After the device restarts, access your router's admin panel (usually at 192.168.8.1).
2. Navigate to DHCP settings and bind your device's name to a static IP address.
3. Restart both your machine and router.

<!-- ![DHCP Configuration](path_to_your_image) -->

### Extra: Preventing Lid Closure Shutdown

Edit the file `/etc/systemd/logind.conf` using nano:

```bash
sudo nano /etc/systemd/logind.conf
```

Change `HandleLidSwitch=ignore`, save the changes, and restart your laptop.

<!-- ![Lid Closure](path_to_your_image) -->

## Step Four: SSH Access

1. Login to the device and switch to root.
2. Run `visudo` and edit user privilege specifications.
3. Restart the laptop.

Access the system remotely using SSH (`ssh username@ip`).

<!-- ![SSH Access](path_to_your_image) -->

## Step Five: Installing Applications

### SMB - Samba

Install the SMB daemon:

```bash
sudo apt install samba
```

Set up Samba to share files over the network.

<!-- ![Samba](path_to_your_image) -->

### Connecting to Your Remote Drive

Map the network drive on your Windows PC using the server's IP address.

<!-- ![Remote Drive](path_to_your_image) -->

## Final Step: Jellyfin

Install Jellyfin:

```bash
curl https://repo.jellyfin.org/install-debuntu.sh | sudo bash
```

Access Jellyfin via `<your_ip>:8096` in a browser to set it up. Install the Jellyfin app on iOS or Android devices to access media remotely.

<!-- ![Jellyfin](path_to_your_image) -->

## Conclusion

Congratulations! You've successfully set up your home server. Explore additional functionalities such as custom DNS servers, VPNs, and local game or website servers.

<!-- ![Conclusion](path_to_your_image) -->
