# Udacity-Linux-Server-Configuration

1. Start a new Ubuntu Linux server instance on Amazon Lightsail. 
2. Follow the instructions provided to SSH into your server.
Save your key to a .pem file and then use it in the following to ssh.
```
ssh -i ~/Downloads/LightsailDefaultPrivateKey-us-east-2.pem grader@13.58.211.203
```
# Secure your server.

3. Update all currently installed packages.
```
sudo apt-get update
sudo apt-get upgrade
```
4. Change the SSH port from 22 to 2200. Make sure to configure the Lightsail firewall to allow it.
```
sudo nano /etc/ssh/sshd_config
sudo service ssh restart
```
5. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123).
```
sudo ufw allow 2200/tcp
sudo ufw allow 80/tcp
sudo ufw allow 123/udp
sudo ufw enable
```

6. Create a new user account named grader.
```
sudo adduser grader
sudo nano /etc/sudoers
touch /etc/sudoers.d/grader
sudo nano /etc/sudoers.d/grader
```
7. Give grader the permission to sudo.
```
sudo nano /etc/sudoers.d/grader
```
Add the following to grader file:
```
grader ALL=(ALL:ALL) ALL
```
8. Create an SSH key pair for grader using the ssh-keygen tool.

# Prepare to deploy your project.
9. Configure the local timezone to UTC.
```
sudo timedatectl set-timezone UTC
```
10. Install and configure Apache to serve a Python mod_wsgi application.
```
sudo apt-get install apache2
```

11. Install and configure PostgreSQL:
```
sudo apt-get install postgresql
```
Do not allow remote connections
Create a new database user named catalog that has limited permissions to your catalog application database.
```
CREATE USER grader WITH PASSWORD grader;
ALTER USER catalog CREATEDB;
CREATE DATABASE catalog WITH OWNER grader;
```
12. Install git.
```
sudo apt-get install git
```

# Deploy the Item Catalog project.

13. Clone and setup your Item Catalog project from the Github repository you created earlier in this Nanodegree program.
```
sudo git clone https://github.com/eschung/udacity-catalogue.git
```
Create a catalog.wsgi file with the following in /var/www/catalogue:
```
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/catalogue/catalogue")

from project import app as application
application.secret_key = 'your secret key'
```
Update your client_secrets.json path to:
```
os.path.dirname(os.path.realpath(__file__))+'/client_secrets.json'
```
14. Set it up in your server so that it functions correctly when visiting your server’s IP address in a browser.
```
18.221.7.66
```
