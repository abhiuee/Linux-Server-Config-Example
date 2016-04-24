Project 5 Configuring Linux Web Servers
=======================================

SSH Port and IP Address
-----------------------

IP 52.38.22.215 SSH Port 2200



Summary of Software Installed and Configuration Changes
-------------------------------------------------------

### User Management

## Root User
For the root user, /etc/ssh/sshd_config file was edit to change PermitRootLogin field to no so that root user is not allowed to ssh. Finger tool was installed to verify the accounts for newly created users.

## Grader User
For grader user, sudo access was provided by using the command sudo visudo and adding user grader. A strong password was created for the user. Key based authentication was allowed and authorized keys was properly set up.

### Security

## SSH Port and misc
SSH Port was edited in /etc/ssh/sshd_config to 2200. All applications were updated using sudo apt-get update and sudo apt-get upgrade commands. Every user is required to ssh using RSA keys only.