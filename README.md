# pwnagotchi-guide

# Hardware

This is what I used for my build

- Raspberry Pi Zero 2 W
- Waveshare v4 e ink
- PiSugar 3
- 32Gb Micro SD card

# Image preparation

## Download:
- [Raspberry Pi Imager](https://downloads.raspberrypi.org/imager/imager_latest.exe)
- [Pwnagotchi 64Bit image](https://github.com/jayofelony/pwnagotchi/releases)
- [RNDIS Windows drivers](https://modclouddownloadprod.blob.core.windows.net/shared/mod-rndis-driver-windows.zip)

## Write image:

Use the Raspberry Pi Imager to write the new image

Apply OS customization settings:

`General`
- Set hostname
- Add a new user and password for that user
- Uncheck wireless lan
- Set the correct locale settings
  
`Services`
- Enable SSH and use password authentication 

# First run

## Windows

Unzip RNDIS driver and right click > install the `RNDIS.inf` driver file.

Connect bare Pi Zero using the data usb port (closest to HDMI port) to a Windows PC.

In Windows `Network Connections`:
- right click > Properties the `main network` to enable sharing.
- right click > Properties the `USB Ethernet/RNDIS Gadget network`. Double click `Internet Protocol Version 4 (TCP/IPv4)` and set the IP address to `10.0.0.1` and the subnet to `255.255.255.0`, leave the rest blank.

## SSH/FTP setup

Connect using ssh
`ssh username@10.0.0.2`
Give root user a password
`sudo passwd root`
Edit sshd config
`sudo vim /etc/ssh/sshd_config`
Uncomment and change the line `#PermitRootLogin prohibit-password` to `PermitRootLogin yes`
Restart ssh server
`sudo service ssh restart`
You can now use sftp to connect and trasfer files
```
host: 10.0.0.2
username: root
password: *password*
port: 22
```
I will be using ssh only for the rest of this guide so no actual need to sftp

## Base system setup

Update and upgrade the OS
```
sudo apt update
sudo apt upgrade
```

Run the pwnagotchi config wizard
`sudo pwnagotchi --wizard`


# Change passwords
Edit/add to `config.toml`
`vim /etc/pwnagotchi/config.toml`
```
ui.web.username = "your_login_user"
ui.web.password = "your_password"
```

# PiSugar
Enbale I2C
```
raspi-config
```
Install power manager
```
cd ~
curl http://cdn.pisugar.com/release/pisugar-power-manager.sh | sudo bash
```
Edit settings, go to [PiSugar Web UI](http://10.0.0.2:8421)
Set the Safe Shutdown values (20%/10s), go to settings and enable `Battery Input Protection` and `Soft Shutdown`. Set `Soft Shutdown Shell` to `Shutdown`
Now update the PiSugar firmware
```
cd ~
curl https://cdn.pisugar.com/release/PiSugarUpdate.sh | sudo bash
```
Lets enable I2C and
