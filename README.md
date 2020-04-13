# (Re)Take control of your datas with Mailinabox and Nextcloud (Raspberry Pie 4 NAS)

## Installation guide

1. [Requirements](#1-requirements)
    * [ISP requirements](#isp-requirements)
    * [Registrar requirements](#registrar-requirements)
    * [Hardware requirements](#hardware-requirements)
2. [OS installation](#2-os-installation)
3. [Hardware installation](#3-hardware-installation)
4. [Initial Ubuntu setup](#4-initial-ubuntu-setup)
    * [Step 1: set up appropriate keyboard layout](#step-1-set-up-appropriate-keyboard-layout)
    * [Step 2: restart your machine to enable changes](#step-2-restart-your-machine-to-enable-changes)
    * [Step 3: allow root login](#step-3-allow-root-login)
    * [Step 4: change username, password and hostname](#step-4-change-username-password-and-hostname)
    * [Step 5: disallow root login](#step-5-disallow-root-login)
    * [Step 6: use a third-party privacy-first public DNS](#step-6-use-a-third-party-privacy-first-public-dns)
5. [Upgrade your system softwares](#5-upgrade-your-system-softwares)
6. [Local network access](#6-local-network-access)
    * [Step 1: display the MAC address of your Pie connected network](#step-1-display-the-mac-address-of-your-pie-connected-network)
    * [Step 2: login to your router admin panel](#step-2-login-to-your-router-admin-panel)
    * [Step 3: register a static address](#step-3-register-a-static-address)
    * [Step 4: SSH access](#step-4-ssh-access)
7. [Remote network access](#7-remote-network-access)
    * [Step 1: set up port forwarding](#step-1-set-up-port-forwarding)
    * [Step 2: disable SMTP blocking](#step-2-disable-smtp-blocking)
    * [Step 3: configure your reverse DNS](#step-3-configure-your-reverse-dns)
8. [Restrict SSH access](#8-restrict-ssh-access)
    * [Step 1: create an SSH key](#step-1-create-an-ssh-key)
    * [Step 2: add your public key to your machine's authorized keys](#step-2-add-your-public-key-to-your-machines-authorized-keys)
    * [Step 3: disallow SSH password authentication](#step-3-disallow-ssh-password-authentication)
9. [Install Mailinabox](#9-install-mailinabox)
10. [Configure your DNS zone](#10-configure-your-dns-zone)
    * [Step 1: access your external DNS configuration](#step-1-access-your-external-dns-configuration)
    * [Step 2: replicate this configuration in your DNS zone](#step-3-replicate-this-configuration-in-your-dns-zone)
11. [Request TLS certificates from Let's Encrypt](#11-request-tls-certificates-from-lets-encrypt)
12. [Configure a RAID1 volume for your datas](#12-configure-a-raid1-volume-for-your-datas)
    * [Step 1: create the RAID1 array](#step-1-create-the-raid1-array)
    * [Step 2: create the filesystem](#step-2-create-the-filesystem)
    * [Step 3: create a mount point](#step-3-create-a-mount-point)
    * [Step 4: mount the filesystem](#step-4-mount-the-filesystem)
    * [Step 5: reassemble the RAID volume automatically on boot](#step-5-reassemble-the-raid-volume-automatically-on-boot)
    * [Step 4: mount the filesystem automatically on boot](#step-4-mount-the-filesystem-automatically-on-boot)
    * [Step 5: move your datas to the RAID volume](#step-5-move-your-datas-to-the-raid-volume)
    * [Step 6: configure email notifications on failures](#step-6-configure-email-notifications-on-failures)
13. [Configure backups](#configure-backups)
    * [Step 1: find a place for your backup machine](#step-1-find-a-place-for-your-backup-machine)
    * [Step 2: set up the backup machine](#step-2-set-up-the-backup-machine)
    * [Step 3: enable the firewall on the backup machine](#step3-enable-the-firewall-on-the-backup-machine)
    * [Step 4: install and configure Fail2ban on the backup machine](#step-4-install-and-configure-fail2ban-on-the-backup-machine)
    * [Step 5: store your backup key in a safe place](#step-6-store-your-backup-key-in-a-safe-place)
    * [Step 6: enable backups](#step-6-enable-backups)

## Maintenance guide

* [Maintenance: hard drive disconnection](#maintenance-hard-drive-disconnection)
  * [Step 1: see RAID status after the disconnection](#step-1-see-raid-status-after-the-disconnection)
  * [Step 2: reconnect your drive](#step-2-reconnect-your-drive)
  * [Step 3: re-add the drive to the RAID volume](#step-3-re-add-the-drive-to-the-raid-volume)
* [Maintenance: hard drive failure](#maintenance-hard-drive-failure)
  * [Step 1: see RAID status after the failure](#step-1-see-raid-status-after-the-failure)
  * [Step 2: disable access to the machine](#step-2-disable-access-to-the-machine)
  * [Step 3: write all disk caches](#step-3-write-all-disk-caches)
  * [Step 4: mark the disk as failed](#step-4-mark-the-disk-as-failed)
  * [Step 5: remove the disk from the RAID](#step-5-remove-the-disk-from-the-raid)
  * [Step 6: replace the disk](#step-6-replace-the-disk)
  * [Step 7: copy the partition table to the new disk](#step-7-copy-the-partition-table-to-the-new-disk)
  * [Step 8: add the new drive to the RAID volume](#step-8-add-the-new-drive-to-the-raid-volume)
  * [Step 9: monitor the mirroring process](#step-9-monitor-the-mirroring-process)
  * [Step 10: re-enable access to the machine](#step-10-re-enable-access-to-the-machine)
* [Maintenance: expanding your RAID volume](#maintenance-expanding-your-raid-volume)
  * [Step 1: see the disks status](#step-1-see-the-disks-status)
  * [Step 2: disable access to the machine before the expanding process](#step-2-disable-access-to-the-machine-before-the-expanding-process)
  * [Step 3: follow the hard drive replacement process for the first drive](#step-3-follow-the-hard-drive-replacement-process-for-the-first-drive)
  * [Step 4: follow the hard drive replacement process for the second drive](#step-4-follow-the-hard-drive-replacement-process-for-the-second-drive)
  * [Step 5: check your RAID volume](#step-5-check-your-raid-volume)
  * [Step 6: remove the bitmap from your RAID array](#step-6-remove-the-bitmap-from-your-raid-array)
  * [Step 7: grow the RAID array](#step-7-grow-the-raid-array)
  * [Step 8: wait until the growing process is completed](#step-8-wait-until-the-growing-process-is-completed)
  * [Step 9: re-add the bitmap to the RAID array](#step-9-re-add-the-bitmap-to-the-raid-array)
  * [Step 10: re-enable access to the machine when it's done](#step-10-re-enable-access-to-the-machine-when-its-done)
* [Maintenance: reset the RAID volume and the disks completely](#maintenance-reset-the-raid-volume-and-the-disks-completely)
  * [Step 1: stop the RAID array](#step-1-stop-the-raid-array)
  * [Step 2: reset the HDDs](#step-2-reset-the-hdds)
  * [Step 3: remove references to the RAID array](#step-3-remove-references-to-the-raid-array)

## 1. Requirements

### ISP requirements

[Back to top ↑](#installation-guide)

In order to host your emails at home, your Internet Service Provider (ISP)
needs to match some requirements:

* Your ISP must give you a static IP address
* Your ISP must allow you to configure your reverse DNS
* Your ISP must not block ports 25 and 465 (SMTP)

In France, the ISP called "[Free](https://free.fr/assistance/54.html)"
matches these requirements.

### Registrar requirements

[Back to top ↑](#installation-guide)

In order to host your emails at home, you'll need a domain name that
you can buy from a domain name registrar.
Your domain name registrar needs to match some requirements:

* Your registrar must offer you to host your DNS zone
* Your registrar must allow you to set up
NS, A, AAAA, SPF, TXT, DKIM, TLSA, SSHFP, SRV, and DMARC records in your DNS zone

The registrar called "[OVH](https://www.ovh.com/fr/order/domain/)"
matches these requirements.

*Note: prefer a domain name that will be dedicated to this usage
(do not use it for other things like web hosting). This is better to
control your sender reputation that will prevent your emails from
being flagged as SPAM.*

### Hardware requirements

[Back to top ↑](#installation-guide)

* 1 × [Raspberry Pie 4 (4 GB RAM)](https://www.kubii.fr/les-cartes-raspberry-pi/2772-nouveau-raspberry-pi-4-modele-b-4gb-kubii-0765756931182.html)
* 1 × [Raspberry Pie 4 case](https://www.kubii.fr/boitiers-et-supports/2681-boitier-officiel-pour-raspberry-pi-4-kubii-3272496298583.html)
* 1 x [Raspberry Pie 4 power supply](https://www.kubii.fr/14-chargeurs-alimentations-raspberry/2678-alimentation-officielle-153w-usb-c-pour-raspberry-pi-4-kubii-3272496300002.html)
* 1 x [SanDisk microSD card 16 Go class 10](https://www.kubii.fr/raspberry-pi-microbit/2587-carte-micro-sd-sandisk-16go-classe10-taux-de-transfert-80mb-kubii-619659161613.html)
* 2 × [Toshiba N300 4 To Hard Drive](https://www.ldlc.com/fiche/PB00259091.html)
* 2 × [Ugreen USB 3.0 external case 50422 for 3,5 inch HDD](https://www.amazon.fr/UGREEN-Bo%C3%AEtier-Externe-Compatible-Alimentation/dp/B076WS2WJ6)
* 1 × [Ugreen ethernet cable Cat 7 10Gbps](https://www.amazon.fr/UGREEN-11260-Ethernet-Nintendo-Consoles/dp/B00QV1F1C4)
* 1 × [Ugreen USB 3.0 SD card reader 30333](https://www.amazon.fr/UGREEN-Lecteur-M%C3%A9moire-CompactFlash-Compatible/dp/B01ANDA8GE/)
(optional if you already have a SD card reader)
* 1 × [Ugreen micro HDMI to HDMI adapter](https://www.amazon.fr/UGREEN-Femelle-Adaptateur-Supporte-Ethernet/dp/B00B2HORKE/)
* 1 × [Ugreen HDMI cable 0,9 m](https://www.amazon.fr/UGREEN-Ethernet-18Gbps-Supporte-Compatible/dp/B07DBYDJQF/)
* 1 × keyboard
* 1 × screen

## 2. OS installation

[Back to top ↑](#installation-guide)

1. Download the [Ubuntu 18.04 64 bits image](https://ubuntu.com/download/raspberry-pi/thank-you?version=18.04.4&architecture=arm64+raspi3)
for Raspberry Pie 4.
2. Put your microSD card in your SD card reader and connect it to your computer.
3. Follow [instructions](https://ubuntu.com/download/raspberry-pi/thank-you)
in order to flash the downloaded image onto the microSD card.
4. Disconnect everything when the process is finished.

## 3. Hardware installation

[Back to top ↑](#installation-guide)

1. Put your microSD card containing the installed OS in your Raspberry Pie.
2. Put your Raspberry Pie into its case.
3. Put your HDDs into their USB 3.0 cases.
4. Connect your Raspberry Pie to your router with the ehernet cable.
5. Connect your drives to your Raspberry Pie with their USB cables.
6. Connect your keyboard and screen to your Raspberry Pie
7. Connect the power adaptators of your drives, Rasberry Pie and screen

Your Ubuntu machine will boot up!

## 4. Initial Ubuntu setup

[Back to top ↑](#installation-guide)

You can login with "ubuntu" as default login and password. You may
experienced login errors if you try to login directly as soon as the
prompt is displayed. This is because some background installations
processes are not completed yet. Wait until SSH keys are displayed on
the screen then press "Enter". You will be prompted to change your
password immediately after login.

*Note: Ubuntu 18.04 for Raspberry Pie 4 is by default using a "qwerty"
keyboard layout which might not be your layout. To prevent loosing access
to your account, I suggest you to set up something universal like
"hellohello" for now, set up appropriate keyboard layout and change
the password later.*

### Step 1: set up appropriate keyboard layout

[Back to top ↑](#installation-guide)

```bash
sudo dpkg-reconfigure keyboard-configuration
```

### Step 2: restart your machine to enable changes

[Back to top ↑](#installation-guide)

```bash
reboot
```

### Step 3: allow root login

[Back to top ↑](#installation-guide)

Because you might want something more personal than "ubuntu" as a
username and hostname, you can change them. We will need the
root account for that so we will temporary allow root login:

```bash
sudo passwd root
logout
```

### Step 4: change username, password and hostname

[Back to top ↑](#installation-guide)

Login as root and use these commands to rename your username,
change your password and your hostname to something more meaningful to you:

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

[Back to top ↑](#installation-guide)

When it's done, you can disallow root login. For security reasons,
you should never leave your root account accessible.

```bash
# Disable root account
passwd -l root

# Reboot to apply changes
reboot
```

### Step 6: use a third-party privacy-first public DNS

[Back to top ↑](#installation-guide)

By default, your machine will use your ISP's DNS server when you connect
it to your router with the ethernet cable.

ISPs do not always use strong encryption on their DNS and they often use
DNS records to track their users’ activity and behavior.

We don't want that for our personal datas, do we? We will use public
DNS from <https://1.1.1.1/dns/> instead:

```bash
echo "nameserver 1.1.1.1
nameserver 1.0.0.1
nameserver 2606:4700:4700::1111
nameserver 2606:4700:4700::1001" | sudo tee /etc/resolv.conf > /dev/null
```

## 5. Upgrade your system softwares

[Back to top ↑](#installation-guide)

This will ensure that all your system softwares are using latest security fixes.

```bash
sudo apt update && sudo apt dist-upgrade -y
```

## 6. Local network access

[Back to top ↑](#installation-guide)

If you want to access your machine from another computer on your local
network instead of directly with a keyboard and a screen, you'll need
to reserve a static IP address for it. If not, the attributed IP address
inside your network will change each time your router starts up, so it's quite annoying.

### Step 1: display the MAC address of your Pie connected network

[Back to top ↑](#installation-guide)

```bash
ifconfig | grep -i ether
```

### Step 2: login to your router admin panel

[Back to top ↑](#installation-guide)

Login to your router according to your ISP and/or router documentation.

*Note: for the "Free" ISP, the URL of the admin panel is: [https://subscribe.free.fr/login/](https://subscribe.free.fr/login/).*

### Step 3: register a static address

[Back to top ↑](#installation-guide)

Register the static IP address according to your ISP and/or router documentation.

The IP address you choose must not be in the DHCP server range.
You can start with something like "192.168.0.101".

*Note: for the "Free" ISP, once logged in, go under
"Ma Freebox" > "Paramétrer mon routeur Freebox" > "Redirections / Baux DHCP"
and fill the form like bellow.*

![register-static-ip](https://user-images.githubusercontent.com/6952638/76153321-a0767700-60ca-11ea-8816-c548047064e4.png)

### Step 4: SSH access

[Back to top ↑](#installation-guide)

With this, you should now be able to access your Raspberry Pie from
your computer (which must be connected to the same network as your Pie)
through SSH with this command:

```bash
ssh <yourUserName>@<yourIpAddress>
```

## 7. Remote network access

### Step 1: set up port forwarding

[Back to top ↑](#installation-guide)

For now, your router is the target of all requests made to your public
IP address, and it does not do anything with them.

We need to instruct it to redirect the traffic to the Pie so that we can
access it from outside the local network, from the Internet.

According to your ISP/router documentation, redirect the traffic from
ports 80, 443, 22, 25, 587, 993, 4190 and 53 to your static local IP address.

*Note: for the "Free" ISP, once logged in, go under
"Ma Freebox" > "Paramétrer mon routeur Freebox" > "Redirections / Baux DHCP"
and fill the form like bellow.*

![port-forwarding](https://user-images.githubusercontent.com/6952638/76295516-0c1c3800-62b5-11ea-98ef-f40c1b95e18f.png)

### Step 2: disable SMTP blocking

[Back to top ↑](#installation-guide)

Your ISP can also block SMTP ports by default to prevent
hijacked computers from sending SPAM.

If it's your case, this will prevent you from sending emails.

According to your ISP/router documentation, disable SMTP blocking.

*Note: for the "Free" ISP, once logged in, go under
"Ma Freebox" > "Blocage du port SMTP sortant".*

![smtp-block](https://user-images.githubusercontent.com/6952638/76679230-08532300-65df-11ea-8a10-2971d531f588.png)

### Step 3: configure your reverse DNS

[Back to top ↑](#installation-guide)

![reverse-dns](https://user-images.githubusercontent.com/6952638/76679325-e6a66b80-65df-11ea-8506-9da1c45869a5.png)

The reverse DNS is the way we can retreive your domain name from your
IP address. You can view the full explanation of what is it on [this blog post](https://www.leadfeeder.com/blog/what-is-reverse-dns-and-why-you-should-care/).

This process is used by anti-spam systems to check if an IP address
associated with a sender address for example (john@example.com)
is related to its domain name (example.com).

The Mailinabox software that we'll install later will use a specific
subdomain to install its stuffs: `box.<yourdomainname>`
and this will be your reverse DNS.

For example, you want to send mail from "john@example.com",
your reverse DNS will be `box.example.com`.

According to your ISP/router documentation, configure your reverse DNS.

*Note: for the "Free" ISP, once logged in, go under
"Ma Freebox" > "Personnaliser mon reverse DNS".*

![reverse-dns](https://user-images.githubusercontent.com/6952638/76679855-2d966000-65e4-11ea-87a5-8c5fe4a8beff.png)

## 8. Restrict SSH access

[Back to top ↑](#installation-guide)

The root account is disabled but now, anybody can potentially access
your machine through your user account if they found your password.

Your user account is not root but have some sudo privileges. So if it's
compromised, an attacker can do pretty much everything he want with your
machine, including accessing your datas.

To protect your account from being accessed by another person that you,
we will disable SSH password authentication and only let your authorized
computers to login with your user account (note that this will not
disable password authentication direcly with a keyboard and a screen connected).

On each computer you want to access your Raspberry Pie with, follow these steps:

### Step 1: create an SSH key

[Back to top ↑](#installation-guide)

If you don't have an SSH key (look for "~/.ssh/id_rsa" and
"~/.ssh/id_rsa.pub" files), use this command to generate one:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

At the prompts, you can press "Enter" to use default settings.

### Step 2: add your public key to your machine's authorized keys

[Back to top ↑](#installation-guide)

From your computer, run:

```bash
ssh <yourUserName>@<yourIpAddress> \
"echo '$(cat ~/.ssh/id_rsa.pub)' | sudo tee -a ~/.ssh/authorized_keys"
```

If you try to reconnect to your machine through SSH, you should now be
able to login without being asked for a password. SSH will automatically
log you if your local SSH key matches one indicated in the remote
"~/.ssh/authorized_keys" file.

### Step 3: disallow SSH password authentication

[Back to top ↑](#installation-guide)

Now that you have an passwordless SSH access to your Raspberry Pie,
we will disallow password authentication. This will prevent all non
authorized computers from being able to access it through SSH.

I recommend you to backup your "~/.ssh/id_rsa" and "~/.ssh/id_rsa.pub"
files in a safe place, for example in a password manager app protected
by a master password.

This will prevent you from loosing access to your Pie if your only
authorized computer dies (in that case, you only have to copy these
files in your next computer to allow connections from it).

To disable SSH password authentication, connect to your Pie and run:

```bash
# Update the config and save the original in a "/etc/ssh/sshd_config.backup" file
sudo sed \
-i'.backup' \
-e 's/PasswordAuthentication yes/PasswordAuthentication no/g' \
/etc/ssh/sshd_config

# Restart SSH
sudo service ssh restart
```

## 9. Install Mailinabox

[Back to top ↑](#installation-guide)

```bash
# Install some dependencies
sudo apt install -y libffi-dev

# Install Mailinabox
curl -s https://mailinabox.email/setup.sh | sudo -E bash
```

During the install process, you will be asked for your domain name and
the main email address that will be set up as the admin account of the system.

When the install process ends, you will be prompted to access your
Mailinabox admin panel through your public IP address:

```text
https://<yourIP>/admin
```

Accept the security warning and login with your credentials.

## 10. Configure your DNS zone

If you go now in the "Status Checks" tab, you will see red issues everywhere.

We did instruct our router to redirect the traffic from our public
IP address to the local IP address of the Pie. But we did not instruct
anybody to redirect traffic from our domain name to our public IP address.

Let's do that.

### Step 1: access your external DNS configuration

[Back to top ↑](#installation-guide)

By default, Mailinabox configure everything to host your DNS
configuration directly on your machine.

This can be an issue in case of a breakdown, because there is no redundancy.
If your Pie dies prematurely, all the instructions regarding to where
your domain name should send your traffic is lost. And reset
everything is not as easy as it sounds.

By experience, I found it safer to host the DNS configuration directly on
the registrar (which has redundancy). If your machine dies or if you want
to host your datas elsewhere, having the configuration hosted on the
registrar side allows you to do this smoothly.

To do that, go in your Mailinabox admin panel, go under
"System > External DNS" to display your external DNS configuration.

![external-dns](https://user-images.githubusercontent.com/6952638/76702663-c94ecb80-66cb-11ea-94a3-496370c4682e.png)

### Step 2: replicate this configuration in your DNS zone

[Back to top ↑](#installation-guide)

Now you need to configure your DNS zone extacly like this. Go into your
registrar admin panel and add all these records according to its
documentation.

This looks like this with OVH:
![dns-zone](https://user-images.githubusercontent.com/6952638/76681053-d34ecc80-65ee-11ea-9232-cdcfe8efaa4a.png)

Even if the modifications are made instantly in the interface, the DNS
configuration can make several hours (up to 24 hours) to be fully
propagated around the world, so wait few hours before continue.

## 11. Request TLS certificates from Let's Encrypt

[Back to top ↑](#installation-guide)

Once your DNS configuration is propagated and OK, you can ask TLS
certificates in order to access your machine with your own domain over HTTPS.

Go under "System > TLS (SSL) Certificates" and hit the "Provision" button
to automatically get a TLS certificates for your domains.

![ssl-certs](https://user-images.githubusercontent.com/6952638/76702693-221e6400-66cc-11ea-9512-626e755d187b.png)

After that, you may see an error. You just need to access your admin
panel directly with your domain name instead of the IP address:

```text
https://box.<yourDomainName>/admin
```

Now, if you go to "Status Checks", you should have green lines everywhere:

![status-checks](https://user-images.githubusercontent.com/6952638/76681225-127e1d00-65f1-11ea-8476-ec94783a4821.png)

*Note: I have one red line on the reverse DNS check because Mailinabox
checks that the reverse DNS is set for both IPV4 and IPV6 but my ISP
only allow me to set up reverse DNS for IPV4 yet. It's not yet and
issue because IPV6 is almost unused for now.*

## 12. Configure a RAID1 volume for your datas

We'll use our machine to host all our personal datas, so we want them to
be safe and redundant. If a hard drive has a failure, we should be able
to replace it without loosing anything. Our 4 TB hard drives will
beautomatically mirrored by our system to provide a unique volume with
4 TB of disk space for our datas.

Ensure your drives are connected and powered-ON and run the following commands.

### Step 1: create the RAID1 array

[Back to top ↑](#installation-guide)

```bash
sudo mdadm --create --verbose /dev/md0 --level=1 --raid-devices=2 /dev/sda /dev/sdb
```

If you see a warning saying "Note: this array has metadata at the start
and may not be suitable as a boot device.". Press "y" and then "Enter",
it is safe to continue.

Your drives will start their mirroring process (even if there is no
data, it's how the RAID1 work). This can take some time to complete,
but the array can be used during this time.

You can monitor the progress by using the following command:

```bash
cat /proc/mdstat
```

### Step 2: create the filesystem

[Back to top ↑](#installation-guide)

Now you have a single volume at /dev/md0, but there is no filesystem
on it, it's still an empty drive and you cannot store anything on it right now.

Create a filesystem in ext4 format on it:

```bash
sudo mkfs.ext4 -F /dev/md0
```

### Step 3: create a mount point

[Back to top ↑](#installation-guide)

Now the file system should be mounted to be used on our machine,
so we need to create a mount point:

```bash
sudo mkdir -p /mnt/md0
```

### Step 4: mount the filesystem

[Back to top ↑](#installation-guide)

Then, mount the filesystem with:

```bash
sudo mount /dev/md0 /mnt/md0
```

You can check the available space by using:

```bash
df -h -x devtmpfs -x tmpfs
```

### Step 5: reassemble the RAID volume automatically on boot

[Back to top ↑](#installation-guide)

We have created manually our RAID volume but it will not be reassembled
automatically after a reboot. To do that, we need to use this command:

```bash
sudo mdadm --detail --scan | sudo tee -a /etc/mdadm/mdadm.conf
```

And to make the RAID array available during the early boot process, use this one:

```bash
sudo update-initramfs -u
```

### Step 4: mount the filesystem automatically on boot

[Back to top ↑](#installation-guide)

We also need to instruct the machine to automatically mount the filesystem on boot:

```bash
echo '/dev/md0 /mnt/md0 ext4 defaults,nofail,discard 0 0' | sudo tee -a /etc/fstab
```

Your RAID volume should now automatically be assembled and mounted on each boot!

### Step 5: move your datas to the RAID volume

[Back to top ↑](#installation-guide)

Right now, your datas are located under "/home/user-data", this is where
Mailinabox stores all your datas: emails, files, settings, certificates...
This folder is located on your micro-SD card, alongside your operating system.
We need to move this folder to the RAID volume in order to have
the benefits of redundancy for them.

```bash
# Move the datas
sudo mv /home/user-data /mnt/md0/

# Create a symbolic link to map the datas from the RAID volume to the micro-SD card
sudo ln -s /mnt/md0/user-data /home/userdata

# Rerun Mailinabox setup to ensure everything is taking infos from the right place
sudo mailinabox
```

### Step 6: configure email notifications on failures

[Back to top ↑](#installation-guide)

The mdadm utility has a feature to email you notifications in case of
failures on your RAID volume (for example, in the case one of your hard drive fails).

Since your machine is already an email server, there is no additional
configuration, just give your email address to mdadm:

```bash
# Configure email address (replace "<yourEmailAddr>"
# by your real email address in the command below)
sudo sed -i'.backup' -e 's/MAILADDR root/MAILADDR <yourEmailAddr>/g' /etc/mdadm/mdadm.conf

# Restart mdadm to apply changes
/etc/init.d/mdadm restart

# Send a test email
sudo mdadm --monitor --scan --test -1
```

## Maintenance: hard drive disconnection

If you disconnect inadvertently one of your hard drive, your machine will not
re-add it in the RAID volume automatically after its reconnection.

### Step 1: see RAID status after the disconnection

[Back to top ↑](#maintenance-guide)

```bash
sudo mdadm -D /dev/md0
```

You'll see something like this:

```text
State : clean, degraded
    Active Devices : 1
   Working Devices : 1
    Failed Devices : 0
     Spare Devices : 0

    Number   Major   Minor   RaidDevice State
       -       0        0        0      removed
       1       8        0        1      active sync   /dev/sda
```

Your RAID volume is in a "degraded" state, because there is only one drive left
active (the other is removed). There is no redundancy anymore but it's still working.

In my case, it's the drive "/dev/sdb" which has been disconnected.

### Step 2: reconnect your drive

[Back to top ↑](#maintenance-guide)

Now reconnect your drive and type:

```bash
lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINT
```

You'll see this:

```text
NAME         SIZE FSTYPE            TYPE  MOUNTPOINT
sda          3.7T linux_raid_member disk  
└─md0
        3.7T ext4              raid1 /mnt/md0
sdb          3.7T linux_raid_member disk  
mmcblk0     29.7G                   disk  
├─mmcblk0p1  256M vfat              part  /boot/firmware
└─mmcblk0p2 29.5G ext4              part  /
```

The drive "sdb" is here, but it's not part of the RAID "md0" anymore.

### Step 3: re-add the drive to the RAID volume

[Back to top ↑](#maintenance-guide)

```bash
sudo mdadm --manage /dev/md0 --add /dev/sdb
```

After that, if you re-type the first command,
your RAID should be fully operational again:

```text
Number   Major   Minor   RaidDevice State
       0       8       16        0      active sync   /dev/sdb
       1       8        0        1      active sync   /dev/sda
```

## Maintenance: hard drive failure

In case of one hard drive failure, your datas will not be lost thanks to RAID.
But we need the replace the failed drive as soon as possible.

### Step 1: see RAID status after the failure

[Back to top ↑](#maintenance-guide)

```bash
sudo mdadm -D /dev/md0
```

You'll see:

```text
Number   Major   Minor   RaidDevice State
    -       0        0        0      removed
    1       8        0        1      active sync   /dev/sda

    0       8       16        -      faulty   /dev/sdb
```

In this case, the "/dev/sdb" drive has a failure.

### Step 2: disable access to the machine

[Back to top ↑](#maintenance-guide)

To preserve the remaining disk, we will disable access to the machine.
**This means you will not be able to receive emails or use your Pie during the process.**

```bash
# Reset all firewall rules
sudo ufw reset

# Only allow SSH (if not, we loose access to the machine)
sudo ufw allow 22

# Reactive the firewall
sudo ufw enable
```

### Step 3: write all disk caches

[Back to top ↑](#maintenance-guide)

```bash
sync
```

### Step 4: mark the disk as failed

[Back to top ↑](#maintenance-guide)

```bash
sudo mdadm --manage /dev/md0 --fail /dev/sdb
```

### Step 5: remove the disk from the RAID

[Back to top ↑](#maintenance-guide)

```bash
sudo mdadm --manage /dev/md0 --remove /dev/sdb
```

If you check the status, it will be:

```text
Number   Major   Minor   RaidDevice State
    -       0        0        0      removed
    1       8        0        1      active sync   /dev/sda
```

### Step 6: replace the disk

[Back to top ↑](#maintenance-guide)

After having the new disk connected, run the command:

```bash
lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINT
```

You'll see:

```text
NAME         SIZE FSTYPE            TYPE  MOUNTPOINT
sda          3.7T linux_raid_member disk  
└─md0        3.7T ext4              raid1 /mnt/md0
sdb          3.7T                   disk  
mmcblk0     29.7G                   disk  
├─mmcblk0p1  256M vfat              part  /boot/firmware
└─mmcblk0p2 29.5G ext4              part  /
```

The new sdb disk is here.

### Step 7: copy the partition table to the new disk

[Back to top ↑](#maintenance-guide)

The RAID system will rebuilt the data to the new HDD but not
the partition table. We need to copy it manually:

```bash
sudo sfdisk -d /dev/sda | sudo sfdisk /dev/sdb
```

You'll see:

```text
Disk /dev/sdb: 3.7 TiB, 4000787030016 bytes, 7814037168 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 33553920 bytes
Disklabel type: gpt
Disk identifier: 43D58C5B-D39A-704C-A36B-E7A065F38EFB

Old situation:

>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Done.
Created a new GPT disklabel (GUID: 7642AC9C-9860-ED4A-A6D1-9CD03FF9137B).

New situation:
Disklabel type: gpt
Disk identifier: 7642AC9C-9860-ED4A-A6D1-9CD03FF9137B

The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```

### Step 8: add the new drive to the RAID volume

[Back to top ↑](#maintenance-guide)

```bash
sudo mdadm --manage /dev/md0 --add /dev/sdb
```

After that, if you check again the status, you'll see:

```text
Number   Major   Minor   RaidDevice State
    2       8       16        0      spare rebuilding   /dev/sdb
    1       8        0        1      active sync   /dev/sda
```

The new drive is beeing rebuilt.

### Step 9: monitor the mirroring process

[Back to top ↑](#maintenance-guide)

```bash
cat /proc/mdstat
```

You'll see:

```text
md0 : active raid1 sdb[2] sda[1]
      3906886464 blocks super 1.2 [2/1] [_U]
      [>....................]  recovery =  0.4% (17419648/3906886464)
      finish=317.4min speed=204184K/sec
      bitmap: 2/30 pages [8KB], 65536KB chunk

unused devices: <none>
```

The recovery is a long process, your RAID volume will be very slow during this time.
That's why we disabled access to the machine, that way, all disk read/write
capacity will be dedicated to the recovery process.

### Step 10: re-enable access to the machine

[Back to top ↑](#maintenance-guide)

```bash
# Reset all firewall rules
sudo ufw reset

# Allow all services
sudo ufw allow 22/tcp
sudo ufw allow 53
sudo ufw allow 25/tcp
sudo ufw allow 587/tcp
sudo ufw allow 993/tcp
sudo ufw allow 995/tcp
sudo ufw allow 4190/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Reactive the firewall
sudo ufw enable
```

## Maintenance: expanding your RAID volume

Here is the case: you have a lot of things to store and you have
reached the limit of your hard drives.
There is almost no available space! You need to replace your drives
one by one by bigger ones in order to keep all the precious data.

### Step 1: see the disks status

[Back to top ↑](#maintenance-guide)

```bash
lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINT
```

For this example, I have a RAID on two disks of 232.9G size.
I want to expand it to a volume of 3.7G size.

```text
NAME          SIZE FSTYPE            TYPE  MOUNTPOINT
sda         232.9G linux_raid_member disk  
└─md0       232.8G ext4              raid1 /mnt/md0
sdb         232.9G linux_raid_member disk  
└─md0       232.8G ext4              raid1 /mnt/md0
mmcblk0      29.7G                   disk  
├─mmcblk0p1   256M vfat              part  /boot/firmware
└─mmcblk0p2  29.5G ext4              part  /
```

The replacement method is simple, we will use the same method required
to replace a failed drive one by one in order to keep our datas.

### Step 2: disable access to the machine before the expanding process

[Back to top ↑](#maintenance-guide)

To preserve the datas during the expanding process,
we will disable access to the machine.
**This means you will not be able to receive emails or use your Pie during the process.**

```bash
# Reset all firewall rules
sudo ufw reset

# Only allow SSH (if not, we loose access to the machine)
sudo ufw allow 22

# Reactive the firewall
sudo ufw enable
```

### Step 3: follow the hard drive replacement process for the first drive

[Back to top ↑](#maintenance-guide)

You can view the process here: [hard drive replacement process](#maintenance-hard-drive-failure).

### Step 4: follow the hard drive replacement process for the second drive

[Back to top ↑](#maintenance-guide)

You can view the process here: [hard drive replacement process](#maintenance-hard-drive-failure).

### Step 5: check your RAID volume

[Back to top ↑](#maintenance-guide)

If you run this command:

```bash
lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINT
```

You'll see that your new drives are here, but the "md0" RAID volume
does not take all available space, it still to the previous size:

```text
NAME          SIZE FSTYPE            TYPE  MOUNTPOINT
sda           3.7T linux_raid_member disk  
└─md0       232.8G ext4              raid1 /mnt/md0
sdb           3.7T linux_raid_member disk  
└─md0       232.8G ext4              raid1 /mnt/md0
mmcblk0      29.7G                   disk  
├─mmcblk0p1   256M vfat              part  /boot/firmware
└─mmcblk0p2  29.5G ext4              part  /
```

We need to grow it!

### Step 6: remove the bitmap from your RAID array

[Back to top ↑](#maintenance-guide)

This is a precaution needed before increasing the size of the array.

```bash
sudo mdadm --grow /dev/md0 --bitmap none
```

### Step 7: grow the RAID array

[Back to top ↑](#maintenance-guide)

```bash
sudo mdadm --grow /dev/md0 --size max
```

This operation can take several hours (depending on the size of the new disks).

### Step 8: wait until the growing process is completed

[Back to top ↑](#maintenance-guide)

You can check the status wih:

```bash
cat /proc/mdstat
```

You'll see:

```text
md0 : active raid1 sdb[3] sda[2]
      3906886488 blocks super 1.2 [2/2] [UU]
      [==>..................]
      resync = 11.1% (436234112/3906886488) finish=289.8min speed=199554K/sec

unused devices: <none>
```

### Step 9: re-add the bitmap to the RAID array

[Back to top ↑](#maintenance-guide)

```bash
sudo mdadm --grow /dev/md0 --bitmap internal
```

Then:

```bash
lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINT
```

You'll see that the "md0" volume takes now all the disk space!

```text
NAME         SIZE FSTYPE            TYPE  MOUNTPOINT
sda          3.7T linux_raid_member disk  
└─md0        3.7T ext4              raid1 /mnt/md0
sdb          3.7T linux_raid_member disk  
└─md0        3.7T ext4              raid1 /mnt/md0
mmcblk0     29.7G                   disk  
├─mmcblk0p1  256M vfat              part  /boot/firmware
└─mmcblk0p2 29.5G ext4              part  /
```

### Step 10: re-enable access to the machine when it's done

[Back to top ↑](#maintenance-guide)

```bash
# Reset all firewall rules
sudo ufw reset

# Allow all services
sudo ufw allow 22/tcp
sudo ufw allow 53
sudo ufw allow 25/tcp
sudo ufw allow 587/tcp
sudo ufw allow 993/tcp
sudo ufw allow 995/tcp
sudo ufw allow 4190/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Reactive the firewall
sudo ufw enable
```

## Maintenance: reset the RAID volume and the disks completely

Sometimes, things mess up, and we don't know why nor how to fix.
In that case, having a fresh start can be necessary.

### Step 1: stop the RAID array

[Back to top ↑](#maintenance-guide)

```bash
sudo mdadm --stop /dev/md0
```

### Step 2: reset the HDDs

[Back to top ↑](#maintenance-guide)

**All datas on these drives will be lost.**

```bash
# Reset "/dev/sda"
echo "g
w" | sudo fdisk /dev/sda

# Reset "/dev/sdb"
echo "g
w" | sudo fdisk /dev/sdb
```

### Step 3: remove references to the RAID array

[Back to top ↑](#maintenance-guide)

```bash
# Remove reference in /etc/fstab
sudo sed -i'.backup' '/\/dev\/md0/d' /etc/fstab

# Remove reference in /etc/mdadm/mdadm.conf
sudo sed -i'.backup' '/\/dev\/md0/d' /etc/mdadm/mdadm.conf

# Apply changes on boot
sudo update-initramfs -u
```
