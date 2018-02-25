<!-- TITLE: How to install a cube -->

This procedure explains how to setup an Internet Cube and configure it for Neutrinet.
It is based on this document:  https://repo.labriqueinter.net/

# What you need

## Neutrinet certs

To use our VPN, you need to follow [this registration process](https://wiki.neutrinet.be/documentation/vpn/order) to generate your private key and obtain your VPN certificate.

## Components

This is the strict minimum you need:
- a computer (preferably running a Linux/Unix/BSD system) to perform the install
- a A20-OLinuXino-LIME board
- a micro SD card where the Cube operating system (Yunohost/Debian) will be stored
- a MOD-WIFI-R5370-ANT WiFi Antenna
- a 5V DC power adapter
- a Neutrinet VPN account (Note : for the moment (until correction), the FQDN of the confirmation link (vpn.neutrinet.be) need to be replace by api.neutrinet.be)

You can find details about the components and buy them here:
https://cube.neutrinet.be

## Others

You'll also need:
- An Internet connection to download the Yunohost image.
- An ethernet cable to connect the Cube to your home router (e.g: your Proximus BBox)

# Install Yunohost on the SD Card

In order to install the latest version of Yunohost on the SD card, we'll use a simple script that does the hard work for you.

## Download the script

Open a terminal, then:
```bash
$ mkdir ~/internet-cube-install
$ cd ~/internet-cube-install
$ wget https://repo.labriqueinter.net/install-sd.sh
$ chmod 0755 install-sd.sh
```

## Run the script

First remove the SD card from your computer if present.

In your terminal, run (add the -2 argument if you use a Lime2 and not a Lime1 and -e if you wan a fully encrypted file system) :
```bash
$ ./install-sd.sh [-e] [-2]
```
And follow the on screen instructions.
It will:
- detect your SD card
- download the latest Yunohost image
- verify its integrity
- install the image on your SD card (all data on the SD card will be lost)

Remove the SD card from your computer when it's done.

# Start your Cube

## Prepare it

- Make sure the Cube is *not* connected to a power source (e.g: the power cable should be unplugged)
- Insert the WiFi antenna into the USB port of the Cube's board
- Insert the SD card into your Cube
- Connect your Cube to an ethernet cable that is connected to your home router (e.g: your Proximus BBox)
- Make sure your computer is connected (via WiFi or ethernet) to your home router too (you will connect to your Cube via the local network)

## Boot it

- Connect the power cable to the Cube to start it
- Wait a couple of minutes to let the Cube start and connect to the network

## Find it on the network

In order to connect to your Cube, you need to find its IP address on your local network.

In your terminal, run:
```bash
$ ./install-sd.sh -l
```

The output should look like this:
```
Internet Cubes found on the network:

  1. YunoHost Admin:    https://192.168.1.46
     SSH Access:        ssh root@192.168.1.46
     HyperCube Debug:   http://192.168.1.46:2468/install.html
```

In this example, the IP address of your Cube on the local network is:
192.168.1.46

If the script cannot find your cube, try again a couple of minutes later.

# Connect to your Cube via SSH

To connect to your Cube as root, in your terminal, run:
```bash
$ ssh root@192.168.1.46
```
(Please replace 192.168.1.46 with the IP address of your Cube found in the previous step)
The root password of your Cube is:
olinux

You'll be asked to change it. Please choose a strong password and do not loose it.

## Configure your Cube for use with Neutrinet

Now that you're connected to your Cube via SSH, you will download the configuration script and run it:
```bash
root@olinux:~# wget https://raw.githubusercontent.com/labriqueinternet/configuration_scripts/master/neutrinet.sh
root@olinux:~# chmod 0755 neutrinet.sh
root@olinux:~# ./neutrinet.sh
```

And follow the on screen instructions.
Important: at some point during the installation, the script will prompt your for an "Administration password". This password is:
neutrinet

It will:
- ask you for:
  - a Main domain name. If you choose example.com, you'll access your cube via https://example.com
  - a Username, used to connect to the user interface and access your apps
  - your Firstname/Lastname, used to send emails
  - an Email. It must contain the domain previously entered as second part, e.g: jon@example.com
  - VPN client certificate. This is the content of the client.crt file you download.
  - VPN client key. This is the content of the client.key file you download.
  - CA server certificate. This is the content of the ca.crt file you download.
  - VPN username/password. Can be found in the auth file you downloaded.
  - IPv6 delegated prefix. If you don't have one, leave it blank (only necessary if you want to use IPv6)
  - WiFi AP SSID. This is the name of the WiFi network created by your Cube, as it will appear in the available WiFi list of your computer.
  - The Administration password. This password is: neutrinet
- update the system
- finish the Yunohost configuration
- install the following Yunohost apps:
  - vpnclient: to connect to the Neutrinet VPN
  - hotspot: to create a WiFi network
  - doctorcube: to help fix the Cube in some situations
  - neutrinet_ynh: to help fix the outdated certificate issue

## Change all passwords

Those passwords have been set to 'neutrinet' (without quotes) during the install process.

- Administration password: https://example.com/yunohost/admin/#/tools/adminpw
- User password: https://example.com/yunohost/sso/password.html
- WiFi Hotspot password: https://example.com/wifiadmin/
- root password:
  ```
  $ ssh root@example.com
  $ passwd
  ``` 

# Configure the DNS records of your domain

See [this page](dns).