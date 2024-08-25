# odoo_custom_rpi5
Report rpi5 and Odoo installations

# rpi5 initialisation
Rpi5 has to be initialized with 

See

Or, instead, in case of a breakage (like me), use online installation when the raspberry starts
- Sometimes the micro SD is unstable (won't boot, for example).
- This has happened with a 64 GB card, whereas a 32 GB card remains stable. 
- Too much memory? Or a construction fault?
If in doubt, I recommend using a 32 GB card.

See guideline here : https://www.raspberrypi.com/documentation/computers/getting-started.html

Allow ssh access and register local IP access to configure/verify your rpi via another terminal

```
#At start, check disk available :
df -h | grep "Avail\|dev/"

```
| Location  | 64 Go  |  32 Go |
|:-----|:--------|:--------|
| Filesystem    |  Size  Used Avail Use% Mounted on   |   Size  Used Avail Use% Mounted on    |
|/dev/mmcblk0p2 |  57G  5.1G   49G  10% /             |   29G  4.9G   23G  18% / |
|tmpfs          | 4.0G   26M  4.0G   1% /dev/shm     |  4.0G   24M  4.0G   1% /dev/shm |
|/dev/mmcblk0p1 | 510M   75M  436M  15% /boot/firmware     | 510M   75M  436M  15% /boot/firmware  |

```
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

# Odoo 16 installation - Without Developpment needs

See : https://community.openems.io/t/how-to-install-odoo-on-a-raspberry-pi/2517

Or instead (other details) : https://fr.cyberaxe.org/article/how-to-install-odoo-on-raspberry-pi-os

Postgres and Odoo will take around 2 Gb space on your hard drive !

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

Continue with your own configuration. 

# Odoo 16 installation - From github source for developpment needs

## Create a specific folder for odoo version 16 (-b XX below)

cd odooLocalPath repo
```
git clone -b 16.0 https://github.com/odoo/odoo.git
```

On Debian/Ubuntu, the following commands should install the required packages:

```
cd odoo         #CommunityPath	where clone was done - Like odooLocalPath here
sudo ./setup/debinstall.sh
```

## Postgresql Install
```
sudo apt install postgresql postgresql-client

sudo -i -u postgres
        postgres@raspberrypi:~$ createuser -d -R -S pi
        postgres@raspberrypi:~$ createdb pi
                exit
```

## Check versions of :
```
python3 --version
        Python 3.11.2
pip3 --version
        pip3 --version
```
Install wkhtmltopdf too :
```
	sudo apt-get -y install wkhtmltopdf
```

Launch odoo instance (You are in odooLocalPath/odoo folder) : 
```
python3 odoo-bin --addons-path=addons -d my_odoo_bdd_name
```

## Test Odoo
Open a webbrowser at : http://localhost:8069

### 1. Set up  admin password

### 2. log into the Odoo database 
	with the base administrator account: use admin as the email and, again, admin as the password.
	http://localhost:8069/
        	admin
		admin
### 3. Exit and test create database
	Aïe :
	 WARNING ? odoo.http: Logged into database 'zizi', but dbfilter rejects it; logging session out. 
        => CTRL+C yo stop odoo instance
   
#### 3.1 Open odoo.conf file and add dbfilter
```	nano odooLocalPath/odoo/odoo.conf 
		[options]
		dbfilter = .*
```
#### 3.2 Launch new instance and Refresh webbrowser
```
	python3 odoo-bin --addons-path=addons -d my_odoo_bdd_name -c odoo.conf
```
	Create db + connexion ok => gma_dev_2
	=> CTRL+C yo stop odoo instance

## Python virtual environnement

### Python requirements

To ignore python libraries errors, install dependancies below before run virtual environnement 
```
sudo apt-get install python3-dev libldap2-dev libsasl2-dev libssl-dev
sudo apt install python3-ldap
sudo apt-get install --reinstall libpq-dev
```
### Activate virtual environnement
```
	pi@raspberrypi:~/odooLocalPath $ python -m venv formation-env
	pi@raspberrypi:~/odooLocalPath $ source formation-env/bin/activate
	(formation-env) pi@raspberrypi:~/odooLocalPath $ python3 --version
		Python 3.11.2
        CTRL + C
```

### Launch odoo as developpement instance from python virtual environnement

```
pi@raspberrypi:~/odooLocalPath cd odoo
pi@raspberrypi:~/odooLocalPath/odoo source ../formation-env/bin/activate
pi@raspberrypi:~/odooLocalPath/odoo python3 odoo-bin -c odoo.conf -d my_odoo_bdd_name

```

# Visual Studio Code installation

See here to install : https://code.visualstudio.com/docs/setup/raspberry-pi
```
sudo apt update
sudo apt install code
```

VS Code will take around 1.5 Gb space on your hard drive !

If VS Code is too slow, try this :
VS Code on Raspberry Pi 4 may be slow with the default setup. A workaround is to disable hardware (GPU) acceleration in VS Code:

    Open the VS Code argv.json file using the Preferences: Configure Runtime Arguments command.
    Set "disable-hardware-acceleration": true.
    Restart VS Code.

The "disable-hardware-acceleration": true runtime argument switch has the effect of passing the --disable-gpu command-line argument on VS Code startup.
Nex


# Open Office

As much as we love Linux distributions, installing libre office on raspbian is a sign of a significant drop in performance.

If you really want to update Word, Excell, PowerPoint or other documents without altering your raspberry equipment, prefer to use Office365 from your web browser... it would be a shame not to take advantage of the licences of friends who have subscribed to W.

That said, if you prefer Office : 

```
sudo apt update
sudo apt install libreoffice
```

# Connecting modules under development to Odoo

Create a folder outside your odooLocalPath : odoo_custom in odooCustomLocalPath.
It's the only way to manage your code without inter off the official odoo repo
```
mkdir odooCustomLocalPath/odoo_custom
```

## Get your own sources :
```
cd odooCustomLocalPath/odoo_custom
git clone https://github.com/your_repo/_source.git
```

## Configure odoo.conf with addons-path
Be sure off the addon path locate in your odooLocalPath and ad it like this :
Doing it this way will avoid errors like : "addons path location not find"
```
nano 
        addons_path = odooLocalPath/odoo/addons, odooCustomLocalPath/odoo_custom
```

## Launch odoo as developpement instance from python virtual environnement with your own modules

```
pi@raspberrypi:~/odooLocalPath cd odoo
pi@raspberrypi:~/odooLocalPath/odoo source ../formation-env/bin/activate
pi@raspberrypi:~/odooLocalPath/odoo python3 odoo-bin -c odoo.conf -d my_odoo_bdd_name -u module_name

```
## From your webBrower, install your modules

Be sure your developper mode is activate
Be sure that apps dependancies are already activate
Reload apps / modules
Activate modules

### Be carreful off Odoo Errors logs.
Sometimes you have to add views step by step so as not to rush an odoo instance on which modules have never worked.
Menu items in others app dependancies in my case.

# Backup of jobs on Raspberry for persistence on another system or Raspberry

See here if you have a USB SD card adapter: [https:// ](https://all3dp.com/2/back-up-raspberry-pi-sd-card/)

Or here if you just have an external hard drive : (not tested at onece) https://www.framboise314.fr/dupliquez-la-carte-sd-de-votre-raspberry-pi-avec-rpi-clone/

