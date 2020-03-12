# (Re)Take control of your datas at home with Mailinabox and Nextcloud on a Raspberry Pie 4 NAS

## ISP requirements

In order to host your emails at home, your Internet Service Provider (ISP) needs to match some requirements:

* Your ISP must give you a static IP address
* Your ISP must allow you to configure your reverse DNS
* Your ISP must not block ports 25 and 465 (SMTP)

In France, the ISP called "[Free](https://free.fr/assistance/54.html)" matches these requirements.

## Hardware requirements

* 1 × [Raspberry Pie 4 (4 GB RAM)](https://www.kubii.fr/les-cartes-raspberry-pi/2772-nouveau-raspberry-pi-4-modele-b-4gb-kubii-0765756931182.html)
* 1 × [Raspberry Pie 4 case](https://www.kubii.fr/boitiers-et-supports/2681-boitier-officiel-pour-raspberry-pi-4-kubii-3272496298583.html)
* 1 x [Raspberry Pie 4 power supply](https://www.kubii.fr/14-chargeurs-alimentations-raspberry/2678-alimentation-officielle-153w-usb-c-pour-raspberry-pi-4-kubii-3272496300002.html)
* 1 x [Toshiba microSD card 16 Go class 10 UHS-I](https://www.kubii.fr/carte-sd-et-stockage/2185-carte-microsd-16gb-classe-10-uhs-i-toshiba-kubii-3272496010963.html)
* 2 × [Toshiba N300 4 To Hard Drive](https://www.ldlc.com/fiche/PB00259091.html)
* 2 × [Ugreen USB 3.0 external case 50422 for 3,5 inch HDD](https://www.amazon.fr/UGREEN-Bo%C3%AEtier-Externe-Compatible-Alimentation/dp/B076WS2WJ6)
* 1 × [Ugreen ethernet cable Cat 7 10Gbps](https://www.amazon.fr/UGREEN-11260-Ethernet-Nintendo-Consoles/dp/B00QV1F1C4)
* 1 × [Ugreen USB 3.0 SD card reader 30333](https://www.amazon.fr/UGREEN-Lecteur-M%C3%A9moire-CompactFlash-Compatible/dp/B01ANDA8GE/) (optional if you already have a SD card reader)
* 1 × [Ugreen micro HDMI to HDMI adapter](https://www.amazon.fr/UGREEN-Femelle-Adaptateur-Supporte-Ethernet/dp/B00B2HORKE/)
* 1 × [Ugreen HDMI cable 0,9 m](https://www.amazon.fr/UGREEN-Ethernet-18Gbps-Supporte-Compatible/dp/B07DBYDJQF/)
* 1 × keyboard
* 1 × screen

## 1. OS installation

1. Download the [Ubuntu 18.04 64 bits image](https://ubuntu.com/download/raspberry-pi/thank-you?version=18.04.4&architecture=arm64+raspi3) for Raspberry Pie 4.
2. Put your microSD card in your SD card reader and connect it to your computer.
3. Follow [instructions](https://ubuntu.com/download/raspberry-pi/thank-you) in order to flash the downloaded image onto the microSD card.
4. Disconnect everything when the process is finished.

## 2. Hardware installation

1. Put your microSD card containing the installed OS in your Raspberry Pie.
2. Put your Raspberry Pie into its case.
3. Put your HDDs into their USB 3.0 cases.
4. Connect your Raspberry Pie to your router with the ehernet cable.
5. Connect your drives to your Raspberry Pie with their USB cables.
6. Connect your keyboard and screen to your Raspberry Pie
7. Connect the power adaptators of your drives, Rasberry Pie and screen

Your Ubuntu machine will boot up!

## 3. Initial Ubuntu setup

You can login with "ubuntu" as default login and password. You may experienced login errors if you try to login directly as soon as the prompt is displayed. This is because some background installations processes are not completed yet. Wait until SSH keys are displayed on the screen then press "Enter". You will be prompted to change your password immediately after login.

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
hostnamectl set-hostname <newHostname>
```

### Step 5: disallow root login

When it's done, you can disallow root login. For security reasons, you should never leave your root account accessible.

```bash
# Disable root account
passwd -l root

# Reboot to apply changes
reboot
```

## 4. Upgrade your system softwares

This will ensure that all your system softwares are using latest security fixes.

```bash
sudo apt update && sudo apt dist-upgrade -y
```

## 5. Wi-Fi network connection (optional)

If you want your Pie to connect to your router through Wi-Fi. Use the following commands:

### Step 1: install NetworkManager

```bash
sudo apt install -y nework-manager
```

### Step 2: start NetworkManager

```bash
sudo service network-manager start
```

### Step 3: list available Wi-Fi networks

```bash
nmcli d wifi list
```

### Step 4: connect to a Wi-Fi network

```bash
nmcli -a d wifi connect <networkName>
```

## 6. Local network access

If you want to access your machine from another computer on your local network instead of directly with a keyboard and a screen, you'll need to reserve a static IP address for it. If not, the attributed IP address inside your network will change each time your router starts up, so it's quite annoying.

Let's fix that.

### Step 1: display the MAC address of your Pie connected network

```bash
ifconfig | grep -i ether
```

### Step 2: login to your router admin panel

Login to your router according to your ISP and/or router documentation.

*Note: for the "Free" ISP, the URL of the admin panel is: [https://subscribe.free.fr/login/](https://subscribe.free.fr/login/).*

### Step 4: register a static address

Register the static IP address according to your ISP and/or router documentation.

The IP address you choose must not be in the DHCP server range. You can start with something like "192.168.0.101".

*Note: for the "Free" ISP, once logged in, go under "Ma Freebox" > "Paramétrer mon routeur Freebox" > "Redirections / Baux DHCP" and fill the form like bellow.*

![register-static-ip](https://user-images.githubusercontent.com/6952638/76153321-a0767700-60ca-11ea-8816-c548047064e4.png)

### Step 4: SSH access

With this, you should now be able to access your Raspberry Pie from your computer (which must be connected to the same network as your Pie) through SSH with this command:

```bash
ssh <yourUserName>@<yourIpAddress>
```

## 7. Remote network access

For now, your router is the target of all requests made to your public IP address, and it does not do anything with them.

We need to instruct it to redirect the traffic to the Pie so that we can access it from outside the local network, from the Internet.

According to your ISP/router documentation, redirect the traffic from ports 80, 443, 22, 25, 587, 993, 4190 and 53 to your static local IP address.

![port-forwarding](https://user-images.githubusercontent.com/6952638/76295516-0c1c3800-62b5-11ea-98ef-f40c1b95e18f.png)

## 8. Restrict SSH access

The root account is disabled but now, anybody can potentially access your machine through your user account if they found your password.

Your user account is not root but have some sudo privileges. So if it's compromised, an attacker can do pretty much everything he want with your machine, including accessing your datas.

To protect your account from being accessed by another person that you, we will disable SSH password authentication and only let your authorized computers to login with your user account (note that this will not disable password authentication direcly with a keyboard and a screen connected).

On each computer you want to access your Raspberry Pie with, follow these steps:

### Step 1: create an SSH key

If you don't have an SSH key (look for "~/.ssh/id_rsa" and "~/.ssh/id_rsa.pub" files), use this command to generate one:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

At the prompts, you can press "Enter" to use default settings.

### Step 2: add your public key to your machine's authorized keys

From your computer, run:

```bash
ssh <yourUserName>@<yourIpAddress> "echo '$(cat ~/.ssh/id_rsa.pub)' >> ~/.ssh/authorized_keys"
```

If you try to reconnect to your machine through SSH, you should now be able to login without being asked for a password. SSH will automatically log you if your local SSH key matches one indicated in the remote "~/.ssh/authorized_keys" file.

### Step 3: disallow SSH password authentication

Now that you have an passwordless SSH access to your Raspberry Pie, we will disallow password authentication. This will prevent all non authorized computers from being able to access it through SSH.

I recommend you to backup your "~/.ssh/id_rsa" and "~/.ssh/id_rsa.pub" files in a safe place, for example in a password manager app protected by a master password.

This will prevent you from loosing access to your Pie if your only authorized computer dies (in that case, you only have to copy these files in your next computer to allow connections from it).

To disable SSH password authentication, connect to your Pie and run:

```bash
# Update the config and save the original in a "/etc/ssh/sshd_config.backup" file
sudo sed -i'.backup' -e 's/PasswordAuthentication yes/PasswordAuthentication no/g' /etc/ssh/sshd_config

# Restart SSH
sudo service ssh restart
```

## 9. Configure a RAID1 volume

We'll use our machine to host all our personal datas, so we want them to be safe and redundant. If a hard drive has a failure, we should be able to replace it without loosing anything. Our 4 TB hard drives will be automatically mirrored by our system to provide a unique volume with 4 TB of disk space for our datas.

Ensure your drives are connected and powered-ON and run the following commands.

### Step 1: create the RAID1 array

```bash
sudo mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sda /dev/sdb
```

If you see a warning saying "Note: this array has metadata at the start and may not be suitable as a boot device.". Press "y" and then "Enter", it is safe to continue.

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

Now the file system should be mounted to be used on our machine, so we need to create a mount point:

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

## 10. Install Mailinabox

```bash
# Install some dependencies
sudo apt install -y libffi-dev

# Install Mailinabox
curl -s https://mailinabox.email/setup.sh | sudo -E bash
```

During the install process, you will be asked for your domain name and the main email address that will be set up as the admin account of the system.
