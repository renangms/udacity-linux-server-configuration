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
* Enable 
```
# Edit ssh config file
$ sudo nano /etc/ssh/sshd_config
```
```
# make the following changes to the file
Port 2200
PermitRootLogin  prohibit-password 
PubkeyAuthentication yes
```
```
$ sudo service ssh restart
```

### Security
* Configure firewall
```
sudo ufw allow 2200/tcp
sudo ufw allow 80/tcp
sudo ufw allow 123/udp
sudo ufw enable
```
* Configure SSH connections 
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
a