# odoo_custom_rpi5
Report rpi5 and Odoo installations

# rpi5 initialisation
Rpi5 has to be initialized with 

See

Or, instead, in case of a breakage (like me), use online installation when the raspberry starts

See guideline here : https://www.raspberrypi.com/documentation/computers/getting-started.html

Allow ssh access and register local IP access to configure/verify your rpi via another terminal

```
#At start, check disk available :
df -h | grep "Avail\|dev/"
    Filesystem      Size  Used Avail Use% Mounted on
    /dev/mmcblk0p2   57G  5.1G   49G  10% /
    tmpfs           4.0G   26M  4.0G   1% /dev/shm
    /dev/mmcblk0p1  510M   75M  436M  15% /boot/firmware

# IP ssh access
pi@raspberrypi:~ $ ifconfig | grep inet
        inet 192.168.1.180  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 2001:861:3941:83b0:b734:2699:4365:5b2e  prefixlen 64  scopeid 0x0<global>
        inet6 fe80::59cf:1b11:882d:4f27  prefixlen 64  scopeid 0x20<link>
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>

# Change password
sudo raspi-config
###  and read espectations

```

# Odoo 16 installation

See : https://community.openems.io/t/how-to-install-odoo-on-a-raspberry-pi/2517

Or instead (other details) : https://fr.cyberaxe.org/article/how-to-install-odoo-on-raspberry-pi-os

```
sudo apt update -y && sudo apt upgrade -y
sudo apt install postgresql -y

# Control installed instance
pi@raspberrypi:~ $ sudo -i -u postgres
    postgres@raspberrypi:~$ psql
    psql (15.8 (Debian 15.8-0+deb12u1))
    Type "help" for help.
  exit
exit

# Purchase installation
wget -O - https://nightly.odoo.com/odoo.key | sudo gpg --dearmor -o /usr/share/keyrings/odoo-archive-keyring.gpg
echo 'deb [signed-by=/usr/share/keyrings/odoo-archive-keyring.gpg] https://nightly.odoo.com/16.0/nightly/deb/ ./' | sudo tee /etc/apt/sources.list.d/odoo.list
sudo apt update -y
sudo apt install odoo -y
```

You should now be able to access the Odoo login page from a browser at http://{ip address}:8069. You can use http://localhost:8069 7 if you don’t know the IP address. You should be able to get the IP address using this command on the Pi:

```
ifconfig
```
From there, you will be asked to set various authentication credentials such as Master Password and Database Name. Be sure to store or write them down as they are critical later on.

Configure your Odoo database : bdd name, password, email, language

Refresh your browser page and see login page to your own Odoo  

Continue with your own configuration. Here as an example

# Open Office
```
sudo apt update
sudo apt install libreoffice
```

# Visual Studio Code installation

See here to install : https://code.visualstudio.com/docs/setup/raspberry-pi
```
sudo apt update
sudo apt install code
```

If VS Code is too slow, try this :
VS Code on Raspberry Pi 4 may be slow with the default setup. A workaround is to disable hardware (GPU) acceleration in VS Code:

    Open the VS Code argv.json file using the Preferences: Configure Runtime Arguments command.
    Set "disable-hardware-acceleration": true.
    Restart VS Code.

The "disable-hardware-acceleration": true runtime argument switch has the effect of passing the --disable-gpu command-line argument on VS Code startup.
Nex


# Backup of jobs on Raspberry for persistence on another system or Raspberry

See here if you have a USB SD card adapter: [https:// ](https://all3dp.com/2/back-up-raspberry-pi-sd-card/)

Or here if you just have an external hard drive : (not tested at onece) https://www.framboise314.fr/dupliquez-la-carte-sd-de-votre-raspberry-pi-avec-rpi-clone/

