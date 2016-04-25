Project 5 Configuring Linux Web Servers
=======================================

SSH Port and IP Address
-----------------------

IP 52.38.22.215 SSH Port 2200

### Web Address
http://ec2-52-38-22-215.us-west-2.compute.amazonaws.com/

Summary of Software Installed and Configuration Changes
-------------------------------------------------------

### Upgrade All Packages 
```
sudo apt-get update
sudo apt-get upgrade
```

### User Management

#### Grader User

* Create grader user ```sudo adduser grader```
* Install finger ```sudo apt-get install``` 
* Give grader sudo access ```sudo visudo```
* Add in this file ```grader ALL=(ALL:ALL) ALL```
* Make .ssh directory in home dir ```mkdir /home/grader/.ssh```
* Copy the authorized_keys from root to grader user ```cp ~/.ssh/authorized_keys /home/grader/.ssh/authorized_keys```
* Change permissions ```chmod 700 .ssh``` ```chmod 644 .ssh/authorized_keys```
* Change ownership ```chown grader .ssh``` ```chown grader .ssh/authorized_keys```

#### Root User

* Remove ssh for root user
  ```
  sudo nano /etc/ssh/sshd_config
  sudo service ssh restart
  ```
  change ```PermitRootLogin``` field to no.

### Time Zone Configuration

* Date verified and timezone is UTC
* The servers and ntp status was also checked using: 
```
ntpq -p
```

### Software Installed

```
apt-get install finger
apt-get install apache2
apt-get install ntp
apt-get install libapache2-mod-wsgi
apt-get install postgresql
apt-get install python-flask python-psycopg2 python-sqlalchemy python-oauth2client
```

### Security

#### SSH Port and misc
* Change port to 2200, ```sudo vim /etc/ssh/sshd_config```
* Change ```PasswordAuthentication``` to no

#### UFW
UFW has been enabled by default, all incoming connections are denied except 80/tcp, 2200/tcp and 123 while all outgoing functions are allowed.

```
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 80/tcp
sudo ufw allow 2200/tcp
sudo ufw allow ntp
sudo ufw status
Status: active 

To 			Action		From
--			------		----
80/tcp		ALLOW		Anywhere
2200/tcp	ALLOW		Anywhere
123			ALLOW		Anywhere
```

### Application
#### Apache
* Install apache ```sudo apt-get install apache2```
* Verify that the default page is shown at the public address
* Install mod_wsgi ```sudo apt-get install libapache2-mod-wsgi```
* Configure Apache ```sudo nano /etc/apache2/sites-enabled/000-default.conf``` by adding line WSGIScriptAlias / /var/www/html/myapp.wsgi
* Restart Apache ```sudo apache2ctl restart```
* To verify if wsgi is enabled ```sudo a2enmod wsgi```

##### Create Catalog App
* Install git ```sudo apt-get install git```
* Clone your repository using ```git clone```
* Follow steps from digital ocean
```
cd /var/www
mkdir catalog
cd catalog
mkdir catalog
mv /home/grader/catalogapp/* /var/www/catalog/catalog/
cd catalog
mv application.py __init__.py
```

##### Configure Apache for Catalog App

```
sudo nano /etc/apache2/sites-available/catalog.conf
```

The catalog.conf file should look like this:

```
<VirtualHost *:80>
  ServerName ec2-52-38-22-215.us-west-2.compute.amazonaws.com
  ServerAdmin admin@ec2-52-38-22-215.us-west-2.compute.amazonaws.com
  ServerAlias ec2-52-38-22-215.us-west-2.compute.amazonaws.com
  WSGIScriptAlias / /var/www/catalog/catalog.wsgi
  <Directory /var/www/catalog/catalog/>
      Order allow,deny
      Allow from all
  </Directory>
  Alias /static /var/www/catalog/catalog/static
  <Directory /var/www/catalog/catalog/static/>
      Order allow,deny
      Allow from all
  </Directory>
  ErrorLog ${APACHE_LOG_DIR}/error.log
  LogLevel warn
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
#### PostgreSQL

* Install postgre ```sudo apt-get install postgresql```
* Add password for user postgres. 
* Edit /etc/postgresql/9.3/main/postgresql.conf and make listen_addresses = 'localhost' so that the remote connections to postgresql are now allowed. This value by default was also localhost.
* Change to postgres user ```sudo -i -u postgres```
* create user catalog ```createuser -W catalog```
* Run psql and verify privilieges to catalog app:
```
psql>> create_database catalogapp;
psql>> GRANT ALL PRIVILEGES ON DATABASE catalogapp to catalog;
psql>>\l
```


