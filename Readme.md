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
### Software Installed

## Apache2
sudo apt-get install apache2
Verified that the web address is able to show Apache  default page

## NTP
sudo apt-get install ntp 
Date verified and timezone is UTC
The servers and ntp status was also checked using: sudo ntpq -p

### User Management

## Root User
For the root user, /etc/ssh/sshd_config file was edit to change PermitRootLogin field to no so that root user is not allowed to ssh. Finger tool was installed to verify the accounts for newly created users.

## Grader User
For grader user, sudo access was provided by using the command sudo visudo and adding user grader. A strong password was created for the user. Key based authentication was allowed and authorized keys was properly set up.

### Security

## SSH Port and misc
SSH Port was edited in /etc/ssh/sshd_config to 2200. All applications were updated using sudo apt-get update and sudo apt-get upgrade commands. Every user is required to ssh using RSA keys only.

## UFW
UFW has been enabled by default, all incoming connections are denied except 80/tcp, 2200/tcp and 123 while all outgoing functions are allowed.
