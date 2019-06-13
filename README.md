#  Linux Server Configuration
#### Udacity - [Full Stack Web Developer Nanodegree](https://www.udacity.com/course/full-stack-web-developer-nanodegree--nd004)

## About
Setup a Linux server with good security settings, a running web server, and deployment of the [Movie Catalog Application](https://github.com/renangms/udacity-item-catalog).
* IP address: 35.176.147.11

## Configuration Steps

### User Management
* Create user *grader*
```
$ adduser grader
```
* Add user *grader* to sudoers
```
$ usermod -aG sudo grader
```
* Generate SSH keys
```
# run this from a local computer
$ ssh-keygen
# copy the public key to the server
$ ssh-copy-id grader@35.176.147.11
```

### Security
* Configure firewall
```
sudo ufw allow 2200/tcp
sudo ufw allow 80/tcp
sudo ufw allow 123/udp
sudo ufw enable
```
* Configure SSH port and authentication
```
# Edit ssh config file
$ sudo vi /etc/ssh/sshd_config
```
```
# make the following changes to the file
Port 2200
PermitRootLogin  no
RSAAuthentication yes
PubkeyAuthentication yes
```
```
$ sudo service ssh restart
```
* Update packages
```
$ sudo apt-get update
$ sudo apt-get upgrade
```

### Application Functionality
* Create directory structure
```
$ cd /var/www
$ sudo mkdir catalog
$ cd catalog
```
* Install git
```
$ sudo apt-get install git
```
* Clone Movie Catalog Application
```
$ git clone https://github.com/renangms/udacity-item-catalog.git catalog
```
* Rename main.py to __init__.py
```
$ mv main.py __init__.py
```
* Change directory and files ownership
```
$ sudo chown -R $USER /var/www/catalog/
```

* Setup virtual environment and install modules
```
$ sudo apt-get install python-pip
$ sudo pip install virtualenv 
$ cd /var/www/catalog/catalog
$ sudo virtualenv venv
$ source venv/bin/activate
$ pip install Flask SQLAlchemy oauth2client requests
```

* Install Apache and  mod_wsgi
```
$ sudo apt-get install apache2 
$ sudo apt-get install libapache2-mod-wsgi
```
* Configure Apache and mod_wsgi
```
$ sudo vi /var/www/catalog/catalog.wsgi
```
```
import sys
sys.path.insert(0, '/var/www/catalog')
from catalog import app as application
```

```
$ sudo vi /etc/apache2/sites-available/catalog.conf
```

```
<VirtualHost *:80>
                ServerName 35.176.147.11
                WSGIScriptAlias / /var/www/catalog/catalog.wsgi
                WSGIDaemonProcess catalog python-path=/var/www/catalog:/var/www/catalog/catalog/venv/lib/python2.7/site-packages
                WSGIProcessGroup catalog
                <Directory /var/www/catalog/catalog/>
                        Order allow,deny
                        Allow from all
                </Directory>
                Alias /static /var/www/catalog/catalog/static
                <Directory /var/www/helloworld/catalog/static/>
                        Order allow,deny
                        Allow from all
                </Directory>
                ErrorLog ${APACHE_LOG_DIR}/error.log
                LogLevel warn
                CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

```
$ sudo sudo a2enmod wsgi 
$ sudo a2ensite catalog
$ sudo service apache2 restart 
```


## References
- [How To Deploy a Flask Application](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)
- [Flask ImportError: No Module Named Flask](https://stackoverflow.com/questions/31252791/flask-importerror-no-module-named-flask)
- [Ubuntu Server packages update message](https://serverfault.com/questions/265410/ubuntu-server-message-says-packages-can-be-updated-but-apt-get-does-not-update)

