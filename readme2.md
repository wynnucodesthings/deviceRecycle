

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

```bash
# Example BIOS setup
sudo nano /etc/bios/settings.conf
```

3. Navigate to the boot menu and set the boot order to prioritize the USB drive.

```bash
# Example boot order setup
sudo nano /etc/boot/order.conf
```

## Step Two: Installation

If successful, you'll boot into the USB drive. Follow these steps:

1. Select the normal installation option.
2. Follow the installation prompts, including setting user and language preferences.
3. When prompted for the local device name, set it to your hostname.local.
4. Choose the appropriate network adapter for your LAN.

```bash
# Example network adapter setup
sudo nano /etc/network/adapter.conf
```

### Partitioning

Select the recommended disk partitioning option; do not enable LVM.

```bash
# Example partitioning setup
sudo nano /etc/partitioning/settings.conf
```

### Completing the Install

Select additional options such as SSH server before completing the installation. Remove the USB drive when the device restarts.

```bash
# Example SSH installation
sudo apt install openssh-server
```

## Step Three: DHCP Configuration

1. After the device restarts, access your router's admin panel (usually at 192.168.8.1).
2. Navigate to DHCP settings and bind your device's name to a static IP address.
3. Restart both your machine and router.

```bash
# Example DHCP configuration
sudo nano /etc/dhcp/dhcp.conf
```

### Extra: Preventing Lid Closure Shutdown

Edit the file `/etc/systemd/logind.conf` using nano:

```bash
sudo nano /etc/systemd/logind.conf
```

Change `HandleLidSwitch=ignore`, save the changes, and restart your laptop.

```bash
# Example logind.conf modification
HandleLidSwitch=ignore
```

## Step Four: SSH Access

1. Login to the device and switch to root.
2. Run `visudo` and edit user privilege specifications.
3. Restart the laptop.

```bash
# Example visudo modification
sudo visudo
```

Access the system remotely using SSH (`ssh username@ip`).

```bash
# Example SSH access
ssh username@192.168.1.1
```

## Step Five: Installing Applications

### SMB - Samba

Install the SMB daemon:

```bash
sudo apt install samba
```

Set up Samba to share files over the network.

```bash
# Example Samba configuration
sudo nano /etc/samba/smb.conf
```

### Connecting to Your Remote Drive

Map the network drive on your Windows PC using the server's IP address.

```bash
# Example mapping network drive
\\192.168.1.1\share
```

## Final Step: Jellyfin

Install Jellyfin:

```bash
curl https://repo.jellyfin.org/install-debuntu.sh | sudo bash
```

Access Jellyfin via `<your_ip>:8096` in a browser to set it up. Install the Jellyfin app on iOS or Android devices to access media remotely.

```bash
# Example Jellyfin setup
http://192.168.1.1:8096
```

## Conclusion

Congratulations! You've successfully set up your home server. Explore additional functionalities such as custom DNS servers, VPNs, and local game or web servers.