# odoo_auto_install
Installation of odoo 15.0 16.0 with virtual env on Ubuntu 22.04

### Features and Benefits

1. Python virtual environment
2. Odoo 15.0 16.0
3. PostgreSQL 12 or 14
4. Multiple instances of Odoo same server
5. ready made configuration file

Simple configuration file to install odoo 15.0 16.0 with virtual env on Ubuntu 22.04

Setting:
```
################################################################################

OE_USER="odoo"
OE_HOME="/$OE_USER"
OE_HOME_EXT="/$OE_USER/${OE_USER}-server"
# The default port where this Odoo instance will run under (provided you use the command -c in the terminal)
# Set to true if you want to install it, false if you don't need it or have it already installed.
INSTALL_WKHTMLTOPDF="True"
# Set the default Odoo port (you still have to use -c /etc/odoo-server.conf for example to use this.)
OE_PORT="8069"
# Choose the Odoo version which you want to install. For example: 16.0, 15.0, 14.0 or saas-22. When using 'master' the master version will be installed.
# IMPORTANT! This script contains extra libraries that are specifically needed for Odoo 16.0
OE_VERSION="16.0"
# Set this to True if you want to install the Odoo enterprise version!
IS_ENTERPRISE="False"
# Installs postgreSQL V14 instead of defaults (e.g V12 for Ubuntu 20/22) - this improves performance
INSTALL_POSTGRESQL="True"
INSTALL_POSTGRESQL_FOURTEEN="True"
# Set this to True if you want to install Nginx!
# Set the superadmin password - if GENERATE_RANDOM_PASSWORD is set to "True" we will automatically generate a random password, otherwise we use this one
OE_SUPERADMIN="admin"
# Set to "True" to generate a random password, "False" to use the variable in OE_SUPERADMIN
GENERATE_RANDOM_PASSWORD="True"
OE_CONFIG="${OE_USER}-server"

# Set the default Odoo longpolling port (you still have to use -c /etc/odoo-server.conf for example to use this.)
LONGPOLLING_PORT="8072"
# Set to "True" to install certbot and have ssl enabled, "False" to use http

GIT_USERNAME="crottolo"
GIT_PASSWORD="you-password-of-github"
################################################################################
```
#### Setting repository
````
## Setting Up Multiple GitHub Repositories

### Public and Private Repository


##### GIT_USERNAME is your username of GitHub for private repository
##### GIT_PASSWORD is your password of GitHub for private repository

```
...
sudo git clone --depth 1 --branch 16.0 https://GIT_USERNAME:GIT_PASSWORD@github.com/crottolo/od_custom_app 

```
# variabile addons_paths
```
sub_dirs=(
  "${OE_HOME}/custom/addons"
  "${OE_HOME_EXT}/addons"
  "${OE_HOME}/custom/free_addons"
  "${OE_HOME}/custom/design-themes"
  "${OE_HOME}/custom/web"
  "${OE_HOME}/custom/social"
  "${OE_HOME}/custom/website"
  "${OE_HOME}/custom/od_custom_app"
  "${OE_HOME}/custom/partner-contact"
)
```
---
````

### Installation

##### 1. Requirements
- Ubuntu 22.04
- 2vCPU and 1GB RAM
- 8GB Disk
This script will work on Ubuntu 22.04, it uses PostgreSQL as database, so it is recommended to run it on a server with at least 1GB of memory. Swap is not required. It will install Odoo 15.0 16.0 with virtual env by default in the home directory of the system user that you specify.

##### 2. Get the script and make it executable

get the install script and make it executable
```
# root user is required

wget https://raw.githubusercontent.com/crottolo/odoo_auto_install/16.0/install_odoo_ent.sh
chmod +x install_odoo_ent.sh
./install_odoo_ent.sh
```

attempt the end of the installation you will see the following message:
```


-----------------------------------------------------------
Done! The Odoo server is up and running. Specifications:
Port: 8069
User service: odoo
Configuraton file location: /etc/odoo-server.conf
Logfile location: /var/log/odoo
User PostgreSQL: odoo
Code location: /odoo
Addons folder: odoo/odoo-server/addons/
Password superadmin (database): dwer324fsdgdfgdg
Start Odoo service: sudo service odoo-server start
Stop Odoo service: sudo service odoo-server stop
Restart Odoo service: sudo service odoo-server restart
-----------------------------------------------------------
``````

in the process you create a user with sudo privileges, for example odoo, and the setup is separate from the root user.

##### 3. Python virtual environment

For view the list o packages installed in the virtual environment:

````
sudo su - odoo
source /odoo/odoo-server/venv/bin/activate
pip list

pip install "package-name-you-want-to-install"

deactivate
````
###### Example:
````
root@odoo_server:~# sudo su odoo
odoo@odoo_server:/root$ cd
odoo@odoo_server:~$ ls
custom  odoo-server  odoo-venv
odoo@odoo_server:~$ source odoo-venv/bin/activate
(odoo-venv) odoo@odoo_server:~$ pip install pandas
Collecting pandas
  Downloading pandas-2.1.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (12.3 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 12.3/12.3 MB 47.6 MB/s eta 0:00:00
Requirement already satisfied: pytz>=2020.1 in ./odoo-venv/lib/python3.10/site-packages (from pandas) (2023.3.post1)
Collecting tzdata>=2022.1
  Downloading tzdata-2023.3-py2.py3-none-any.whl (341 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 341.8/341.8 KB 125.2 MB/s eta 0:00:00
Collecting numpy>=1.22.4
  Downloading numpy-1.26.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (18.2 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 18.2/18.2 MB 54.6 MB/s eta 0:00:00
Collecting python-dateutil>=2.8.2
  Downloading python_dateutil-2.8.2-py2.py3-none-any.whl (247 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 247.7/247.7 KB 109.6 MB/s eta 0:00:00
Requirement already satisfied: six>=1.5 in ./odoo-venv/lib/python3.10/site-packages (from python-dateutil>=2.8.2->pandas) (1.16.0)
Installing collected packages: tzdata, python-dateutil, numpy, pandas
  Attempting uninstall: python-dateutil
    Found existing installation: python-dateutil 2.8.1
    Uninstalling python-dateutil-2.8.1:
      Successfully uninstalled python-dateutil-2.8.1
Successfully installed numpy-1.26.0 pandas-2.1.1 python-dateutil-2.8.2 tzdata-2023.3
(odoo-venv) odoo@odoo_server:~$ deactivate 
odoo@odoo_server:~$ 
````

the important thing is to activate the virtual environment before installing the packages and then deactivate it.
you see the confirm activation of the virtual environment in the prompt 
***(odoo-venv) odoo@odoo_server:*** pip install pandas
and the deactivation in the prompt 
***(odoo@odoo_server:~$)***

after the installation of the package you can deactivate the virtual environment.
````
(odoo-venv) odoo@odoo_server:~$ deactivate
odoo@odoo_server:~$ 
````

##### 4. check ip address of the server

````
curl ifconfig.me
````
##### 5. create a database on ip address of the server

````
http://ip-address:8069/web/database/manager
````
##### 6. Conclusion

You've successfully installed Odoo 15.0/16.0 with a Python virtual environment on Ubuntu 22.04. This setup allows you to run multiple instances on the same server and offers a ready-made configuration for quick deployment.

If you found this script helpful, consider giving it a "like" on its GitHub repository. For more content like this, subscribe and hit the "like" button on the CrottoCode YouTube channel.

- **GitHub Repository**: [odoo_auto_install](https://github.com/crottolo/odoo_auto_install)
- **YouTube Channel**: [CrottoCode](https://youtube.com/@CrottoCode?si=JQqVblSkvNBBdC5S)

Your support helps in creating more such helpful content. Thank you!

Issues:

after installation with enterprise flag, you can have a problem with 