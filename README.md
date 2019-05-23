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
$ sudo nano /etc/ssh/sshd_config
```
```
# make the following changes to the file
Port 2200
PermitRootLogin  prohibit-password 
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
* Install git
```
$ sudo apt-get install git
```
* Clone Movie Catalog Application
```
$ cd /var/www
$ git clone https://github.com/renangms/udacity-item-catalog.git movies
```
* Install Apache and  mod_wsgi
```
$ sudo apt-get install apache2 and 
$ sudo apt-get install libapache2-mod-wsgi
```
* Configure Apache and mod_wsgi
```
$ sudo vi /var/www/movies/movies.wsgi
```

```
import sys
sys.path.insert(0, '/var/www/movies')
from movies import app as application
```

```
$ sudo vi /etc/apache2/sites-available/movies.conf
```

```
<virtualHost *:80>
        ServerName 35.176.147.11

        WSGIScriptAlias / /var/www/movies.wsgi

        <Directory /var/www/movies/>
                Order deny,allow
                Allow from all
        </Directory>
</VirtualHost>
```

```
```
