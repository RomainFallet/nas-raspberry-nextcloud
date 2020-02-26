# NAS Raspberry Nextcloud

## Hardware requirements

* 1 × [Raspberry Pie 4 (4 GB RAM)](https://www.kubii.fr/les-cartes-raspberry-pi/2772-nouveau-raspberry-pi-4-modele-b-4gb-kubii-0765756931182.html)
* 1 × [Raspberry Pie 4 case](https://www.kubii.fr/boitiers-et-supports/2681-boitier-officiel-pour-raspberry-pi-4-kubii-3272496298583.html)
* 1 x [Raspberry Pie 4 power supply](https://www.kubii.fr/14-chargeurs-alimentations-raspberry/2678-alimentation-officielle-153w-usb-c-pour-raspberry-pi-4-kubii-3272496300002.html)
* 1 x [Toshiba microSD card 16 Go class 10 UHS-I](https://www.kubii.fr/carte-sd-et-stockage/2185-carte-microsd-16gb-classe-10-uhs-i-toshiba-kubii-3272496010963.html)
* 2 × [Toshiba N300 4 To Hard Drive](https://www.ldlc.com/fiche/PB00259091.html)
* 2 × [Ugreen USB 3.0 external case 50422 for 3,5 inch HDD](https://www.amazon.fr/UGREEN-Bo%C3%AEtier-Externe-Compatible-Alimentation/dp/B076WS2WJ6)
* 1 × [Ugreen ethernet cable Cat 7 10Gbps](https://www.amazon.fr/UGREEN-11260-Ethernet-Nintendo-Consoles/dp/B00QV1F1C4)
* 1 × [Ugreen USB 3.0 SD card reader 30333](https://www.amazon.fr/UGREEN-Lecteur-M%C3%A9moire-CompactFlash-Compatible/dp/B01ANDA8GE/) (optional if you already have a SD card reader)
* 1 × [Ugreen micro HDMI to HDMI adaptor](https://www.amazon.fr/UGREEN-Femelle-Adaptateur-Supporte-Ethernet/dp/B00B2HORKE/)
* 1 × [Ugreen HDMI cable](https://www.amazon.fr/UGREEN-Ethernet-18Gbps-Supporte-Compatible/dp/B07DBYDJQF)
* 1 × HDMI compatible screen
* 1 × USB compatible keyboard

## 1. OS installation

1. Download the [Ubuntu 18.04 64 bits image](https://ubuntu.com/download/raspberry-pi/thank-you?version=18.04.4&architecture=arm64+raspi3) for Raspberry Pie 4.
2. Put your microSD card in your SD card reader and connect it to your computer.
3. Follow [instructions](https://ubuntu.com/download/raspberry-pi/thank-you) in order to flash the downloaded image onto the microSD card.
4. Disconnect everything when the process is finished.

## 2. Hardware installation

1. Put your microSD card containing the installed OS in your Raspberry Pie.
2. Put your Raspberry Pie into its case.
3. Put your HDDs into their USB 3.0 cases.
4. Connect your drives to your Raspberry Pie with their USB cables.
5. Connect your keyboard to your Raspberry Pie with its USB cable.
6. Connect your Raspberry Pie to your screen with the micro HDMI and HDMI cables.
7. Connect the power adaptators of your drives, Rasberry Pie and screen.

Your Ubuntu machine will boot up! Connecting to the screen and keyboard is only required for the initial setup.

## 3. Initial Ubuntu setup

You can login with "ubuntu" as default login and password. On the first time, you may experienced login errors if you try to login directly as soon as the prompt is displayed. This is because some background installations processes are not completed yet. Wait until SSH keys are displayed on the screen then press "Enter". You will be prompted to change your password immediately after login.

*Note: Ubuntu 18.04 for Raspberry Pie 4 is by default using a "qwerty" keyboard layout which might not be your layout. To prevent loosing access to your account, I suggest you to set up something universal like "hellohello" for now, set up appropriate keyboard layout and change the password later.*

### Step 1: set up appropriate keyboard layout

```bash
sudo dpkg-reconfigure keyboard-configuration
```

### Step 2: restart your machine to enable changes

```bash
reboot
```

### Step 3: allow root login

Because you might want something more personal than "ubuntu" as a username and hostname, you can change them. We will need the root account for that so we will temporary allow root login:

```bash
sudo passwd root
logout
```

### Step 4: change username, password and hostname

Login as root and use these commands to rename your username, change your password and your hostname to something more meaningful to you:

```bash
# Change username
usermod -l <newUserName> ubuntu

# Rename home directory
usermod -d /home/<newUserName> -m <newUserName>

# Change password
passwd <newUserName>

# Change hostname
echo '<newHostName>' > /etc/hostname
```

### Step 5: disallow root login

When it's done, you can disallow root login. For security reasons, you should never leave your root account accessible.

```bash
passwd -l root
logout
```

## 4. Local network access

If you want to access your machine from another computer of your local network instead of directly with a keyboard and a screen, you'll need to reserve a static IP address for it inside your network. If not, the attributed IP address will change each time your router starts up, so it's quite annoying. The configuration for this is handled by your router.

1. Connect your Raspberry Pie to your routeur with the ethernet cable.
2. According to the documentation of your router or your ISP provider, reserve a static IP address for it.

For example, I've configured mine like this with my [TP-Link Archer MR600 router](https://www.tp-link.com/my/home-networking/mifi/archer-mr600/):

![Capture du 2020-02-25 18-00-31](https://user-images.githubusercontent.com/6952638/75269872-ec3b3d80-57f9-11ea-9b4e-d4cf64e29ef4.png)

With this, you should now be able to access your Raspberry Pie through SSH with this command:

```bash
ssh <yourUserName>@<yourIpAddress>
```

In my case, my local IP address for my machine is 192.168.1.101.

## 5. Configure a RAID1 volume

We'll use our machine to host all our personal datas, so we want them to be safe and redundant. If a hard drive has a failure, we should be able to replace it without loosing anything. Our 4 TB hard drives will be automatically mirrored by our system to provide a unique volume with 4 TB of disk space for our datas.

Ensure your drives are connected and powered-ON and run the following commands.

### Step 1: create the RAID1 array

```bash
sudo mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sda /dev/sdb
```

If you see a warning saying by "Note: this array has metadata at the start and may not be suitable as a boot device.". Press "y" and then "Enter", it is safe to continue.

Your drives will start their mirroring process (even if there is no data, it's how the RAID1 work). This can take some time to complete, but the array can be used during this time.

You can monitor the progress by using the following command:

```bash
cat /proc/mdstat
```

### Step 2: create the filesystem

Now you have a single volume at /dev/md0, but there is no filesystem on it, it's still an empty drive and you cannot store anything on it right now.

Create a filesystem in ext4 format on it:

```bash
sudo mkfs.ext4 -F /dev/md0
```

### Step 3: create a mount point

Now the file system should be mounted to be used on our machine, so we need to creae a mount point:

```bash
sudo mkdir -p /mnt/md0
```

### Step 4: mount the filesystem

Then, mount the filesystem with:

```bash
sudo mount /dev/md0 /mnt/md0
```

You can check the available space by using:

```bash
df -h -x devtmpfs -x tmpfs
```

### Step 5: reassemble the RAID volume automatically on boot

We have created manually our RAID volume but it will not be reassembled automatically after a reboot. To do that, we need to use this command:

```bash
sudo mdadm --detail --scan | sudo tee -a /etc/mdadm/mdadm.conf
```

And to make the RAID array available during the early boot process, use this one:

```bash
sudo update-initramfs -u
```

### Step 4: mount the filesystem automatically on boot

We also need to instruct the machine to automatically mount the filesystem on boot:

```bash
echo '/dev/md0 /mnt/md0 ext4 defaults,nofail,discard 0 0' | sudo tee -a /etc/fstab
```

Your RAID volume should now automatically be assembled and mounted on each boot!
