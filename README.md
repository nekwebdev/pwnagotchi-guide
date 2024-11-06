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

## SSH/SFTP setup

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

## Configuration

Run the config wizard but only set the pwnagotchi name

`sudo pwnagotchi --wizard`

First stop the pwnagotchi service

`sudo service pwnagotchi stop`

Edit/Create the config.toml file that should only contain the `main.name` entry.

`sudo vim /etc/pwnagotchi/config.toml`

Make sure to edit those parameters correctly: `main.whitelist` `main.plugins.grid.exclude` `main.wpa-sec.api_key` `ui.web.username/password`

```
main.lang = "en"

main.custom_plugins = "/etc/pwnagotchi/custom-plugins"
main.confd = "/etc/pwnagotchi/conf.d/"
main.custom_plugin_repos = [
 "https://github.com/jayofelony/pwnagotchi-torch-plugins/archive/master.zip",
 "https://github.com/Sniffleupagus/pwnagotchi_plugins/archive/master.zip",
 "https://github.com/NeonLightning/pwny/archive/master.zip",
 "https://github.com/nullm0ose/pwnagotchi-plugin-pisugar3/archive/master.zip",
 "https://github.com/Kaska89/pwnagotchi-EXPv2-plugin/archive/master.zip",
]

main.whitelist = [
 "EXAMPLE_NETWORK",
 "ANOTHER EXAMPLE NETWORK",
 "fo:od:ba:de:fo:od",
 "fo:od:ba",
]

main.plugins.grid.enabled = false
main.plugins.grid.report = false
main.plugins.grid.exclude = [
 "EXAMPLE_NETWORK",
 "ANOTHER EXAMPLE NETWORK",
 "fo:od:ba:de:fo:od",
 "fo:od:ba",
]

main.plugins.bt-tether.enabled = false
main.plugins.bt-tether.devices.ios-phone.enabled = false
main.plugins.bt-tether.devices.ios-phone.mac = ""
main.plugins.bt-tether.devices.ios-phone.search_order = 1
main.plugins.bt-tether.devices.ios-phone.ip = "172.20.10.6"
main.plugins.bt-tether.devices.ios-phone.netmask = 24
main.plugins.bt-tether.devices.ios-phone.interval = 1
main.plugins.bt-tether.devices.ios-phone.scantime = 10
main.plugins.bt-tether.devices.ios-phone.max_tries = 10
main.plugins.bt-tether.devices.ios-phone.share_internet = true
main.plugins.bt-tether.devices.ios-phone.priority = 999

main.plugins.bt-tether.devices.android-phone.enabled = false
main.plugins.bt-tether.devices.android-phone.search_order = 1
main.plugins.bt-tether.devices.android-phone.mac = ""
main.plugins.bt-tether.devices.android-phone.ip = "192.168.44.44"
main.plugins.bt-tether.devices.android-phone.netmask = 24
main.plugins.bt-tether.devices.android-phone.interval = 1
main.plugins.bt-tether.devices.android-phone.scantime = 10
main.plugins.bt-tether.devices.android-phone.max_tries = 10
main.plugins.bt-tether.devices.android-phone.share_internet = true
main.plugins.bt-tether.devices.android-phone.priority = 1

main.plugins.clock.enabled = true

main.plugins.display-password.enabled = true
main.plugins.display-password.orientation = "horizontal"

main.plugins.enable_assoc.enabled = true
main.plugins.enable_assoc.position = "206,110,30,59"

main.plugins.enable_deauth.enabled = true
main.plugins.enable_deauth.position = "187,110,30,59"

main.plugins.expv2.enabled = true
main.plugins.expv2.lvl_x_coord = 139
main.plugins.expv2.lvl_y_coord = 60
main.plugins.expv2.exp_x_coord = 170
main.plugins.expv2.exp_y_coord = 60
main.plugins.expv2.str_x_coord = 84
main.plugins.expv2.str_y_coord = 20
main.plugins.expv2.bar_symbols_count = 9

main.plugins.handshakes-dl.enabled = true

main.plugins.internet-connection.enabled = true

main.plugins.IPDisplay.enabled = true
main.plugins.IPDisplay.skip_devices = [ "lo",]

main.plugins.memtemp-plus.enabled = true
main.plugins.memtemp-plus.scale = "celsius"
main.plugins.memtemp-plus.orientation = "horizontal"
main.plugins.memtemp-plus.fields = [ "mem,cpu,temp,freq",]
main.plugins.memtemp-plus.position = "159,70"
main.plugins.memtemp-plus.linespacing = 12

main.plugins.pisugar3.enabled = true
main.plugins.pisugar3.shutdown = 15

main.plugins.wpa-sec.enabled = true
main.plugins.wpa-sec.api_key = "key"
main.plugins.wpa-sec.api_url = "https://wpa-sec.stanev.org"
main.plugins.wpa-sec.download_results = false

main.plugins.tweak_view.enabled = true

ui.display.enabled = true
ui.display.type = "waveshare_4"
ui.display.rotation = 180

ui.invert = true
ui.web.username = "changeme"
ui.web.password = "changeme"
```

Restart the pwnagotchi service

`sudo service pwnagotchi start`

## Plugins

Create the plugin folder

`sudo mkdir /etc/pwnagotchi/custom-plugins`

Install new plugins

```
sudo pwnagotchi update
sudo pwnagotchi install clock
sudo pwnagotchi install display-password
sudo pwnagotchi install enable_assoc
sudo pwnagotchi install enable_deauth
sudo pwnagotchi install expv2
sudo pwnagotchi install handshakes-dl
sudo pwnagotchi install internet-connection
sudo pwnagotchi install IPDisplay
sudo pwnagotchi install memtemp-plus
sudo pwnagotchi install pisugar3
sudo pwnagotchi install tweak_view
```

Restart the pwnagotchi service

`pwnkill`

You can now mount the LCD and PiSugar battery pack.

## PiSugar 3 config

Power the pwnagotchi with the PiSugar3, then connect to Windows PC after it boots

Enable I2C
```
raspi-config
```

Install power manager
```
cd ~
curl http://cdn.pisugar.com/release/pisugar-power-manager.sh | sudo bash
```

Edit settings, go to [PiSugar Web UI](http://10.0.0.2:8421)

Set the Safe Shutdown values (20%/10s), go to settings and enable `Battery Input Protection` and `Soft Shutdown`. Set `Long Tap` to `Shutdown` and `Soft Shutdown Shell` to `Shutdown` 

Now update the PiSugar firmware
```
cd ~
curl https://cdn.pisugar.com/release/PiSugarUpdate.sh | sudo bash
```

If it crashes, burn a new SD, do the first boot with the PiSugar3 disconnected, be extra careful not to short it as it tends to stay live when bricked. Once the system is done booting, power off, connect the PiSugar3 and restart. Try the update process again, if it hangs, use the reset buttons close to the battery level leds. I had it happen and that fixed it.

Setup the RTC in the boot.txt

```
# Enable real-time clock
dtoverlay=i2c-rtc,ds3231
```
