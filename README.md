# Udacity-Linux-Configuration
Linux configuration of udacity full stack course

1 The IP address and SSH port can be accessed by the reviewer at http://13.232.172.47 and SSH at 2200
2 The complete URL to your hosted web application. http://13.232.172.47/
3 A summary of software you installed and configuration changes made.

 -Setup the Amazon Lightsail as mentioned in:
https://classroom.udacity.com/nanodegrees/nd004-intr2/parts/d21e4093-10a7-4ef3-a56f-4623e05161b0/modules/e5bd1d0f-22bc-   45bd-b512-4c59d7220ae9/lessons/3573679011239847/concepts/c4cbd3f2-9adb-45d4-8eaf-b5fc89cc606e

- updated the software using 
sudo apt-get update
sudo apt-get upgrade

-sudo ufw status

sudo ufw status
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 2200
sudo ufw allow 2200/tcp
sudo ufw allow 80
sudo ufw allow 80/tcp
sudo ufw allow 123
sudo ufw allow 123/tcp
sudo ufw enable
sudo ufw status

- Updating timezone
sudo dpkg-reconfigure tzdata

- Updating the server with essentials
sudo apt-get install python-pip
sudo apt-get install python-flask
sudo apt-get install apache2
sudo apt-get install libapache2-mod-wsgi
sudo apt-get install python-XXX
Note: replace XXX with the name of the library you are installing

-Install and configuring GIT
sudo apt-get update
sudo apt-get install git
cd /var/www git clone <git-url>

-create a .wsgi file
nano itemsCatalog.wsgi

-WSGI file would contain:
WSGI file

import sys
import logging

logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, '/var/www/itemsCatalog/vagrant/catalog')

from application import app as application
application.secret_key='super_secret_key'


-Create a new itemsCatalog.conf using cd /etc/apache2/sites-available/itemsCatalog.conf 

Virtual Host file
<VirtualHost *:80>
     ServerName  PublicIP
     ServerAdmin email address
     #Location of the items-catalog WSGI file
     WSGIScriptAlias / /var/www/itemsCatalog/vagrant/catalog/itemsCatalog.wsgi
     #Allow Apache to serve the WSGI app from our catalog directory
     <Directory /var/www/itemsCatalog/vagrant/catalog>
          Order allow,deny
          Allow from all
     </Directory>
     #Allow Apache to deploy static content
     <Directory /var/www/itemsCatalog/vagrant/catalog/static>
        Order allow,deny
        Allow from all
     </Directory>
      ErrorLog ${APACHE_LOG_DIR}/error.log
      LogLevel warn
      CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>


Installing POSTGRESQL

sudo apt-get install postgresql
sudo su postgres
Type psql
postgres=# CREATE USER catalog WITH PASSWORD 'catalog';
postgres=# ALTER USER catalog CREATEDB;
postgres=# CREATE DATABASE catalog WITH OWNER catalog;
catalog=# REVOKE ALL ON SCHEMA public FROM public;
catalog=# GRANT ALL ON SCHEMA public TO catalog;
postgres=# \q
postgres@ip-10-20-11-110~$ exit

-Change project.py and in database_setup.py: engine = create_engine('postgresql://catalog:catalog@localhost/catalog')

-Change Gconnect to absolute path:
example : app_token = json.loads(
	open(r'/var/www/itemsCatalog/client_secret.json', 'r').read())['web']['client_id']


iv. A list of any third-party resources you made use of to complete this project.

https://hk.saowen.com/a/0a0048ca7141440d0553425e8df46b16cdf4c13f50df4c5888256393d34bb1b9
https://medium.com/@hvedam/aws-light-sail-and-flask-25f093081ea
https://github.com/harushimo/linux-server-configuration
https://github.com/ghoshabhi/P5-Linux-Config

P.s: The login functionality wont work because I am using the http://13.232.172.47 as URL
https://stackoverflow.com/questions/36020374/google-permission-denied-to-generate-login-hint-for-target-domain-not-on-localh
