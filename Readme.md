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

#### Root User

* Remove ssh for root user
  ```
  sudo nano /etc/ssh/sshd_config
  ```
  Change PermitRootLogin field to no.

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
SSH Port was edited in /etc/ssh/sshd_config to 2200. Every user is required to ssh using RSA keys only.

#### UFW
UFW has been enabled by default, all incoming connections are denied except 80/tcp, 2200/tcp and 123 while all outgoing functions are allowed.

```
sudo ufw status
Status: active 

To 			Action		From
--			------		----
80/tcp		ALLOW		Anywhere
2200/tcp	ALLOW		Anywhere
123			ALLOW		Anywhere
```

### Application

#### PostgreSQL

##### Security
Add password for user postgres. Edit /etc/postgresql/9.3/main/postgresql.conf and make listen_addresses = 'localhost' so that the remote connections to postgresql are now allowed. This value by default was also localhost.

##### Catalog User
Following steps were taken:

```
sudo -i -u postgres
createuser -W catalog
psql>> create_database catalogapp;
psql>> GRANT ALL PRIVILEGES ON DATABASE catalogapp to catalog;
psql>>\l
```

\l was used to verify that the privileges have been provided to catalog user.


