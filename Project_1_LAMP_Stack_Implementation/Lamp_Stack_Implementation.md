
# Create an Ubuntu Server 20.04 LTS (HVM)

# Change permissions for the private key file (.pem)-
chmod 400 devops_intern.pem

# Connect to Ubuntu Instance- 
    ssh -i "devops_intern.pem" ubuntu@ec2-18-222-228-181.us-east-2.compute.amazonaws.com

# Update a list of packages in package manager
    ubuntu@ip-172-31-31-44:~$ sudo apt update

    Hit:1 http://us-east-2.ec2.archive.ubuntu.com/ubuntu jammy InRelease
    Hit:2 http://us-east-2.ec2.archive.ubuntu.com/ubuntu jammy-updates InRelease
    Hit:3 http://us-east-2.ec2.archive.ubuntu.com/ubuntu jammy-backports InRelease
    Hit:4 http://security.ubuntu.com/ubuntu jammy-security InRelease
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    38 packages can be upgraded. Run 'apt list --upgradable' to see them.
    ubuntu@ip-172-31-31-44:~$
  
# Install Apache
ubuntu@ip-172-31-31-44:~$ sudo apt install apache2

Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  apache2-bin apache2-data apache2-utils bzip2 libapr1 libaprutil1 libaprutil1-dbd-sqlite3
  libaprutil1-ldap liblua5.3-0 mailcap mime-support ssl-cert
Suggested packages:
  apache2-doc apache2-suexec-pristine | apache2-suexec-custom www-browser bzip2-doc
The following NEW packages will be installed:
  apache2 apache2-bin apache2-data apache2-utils bzip2 libapr1 libaprutil1 libaprutil1-dbd-sqlite3
  libaprutil1-ldap liblua5.3-0 mailcap mime-support ssl-cert
0 upgraded, 13 newly installed, 0 to remove and 38 not upgraded.
Need to get 2138 kB of archives.
After this operation, 8501 kB of additional disk space will be used.
Do you want to continue? [Y/n] y


# Verify Apache is running as a service 
ubuntu@ip-172-31-31-44:~$ sudo systemctl status apache2

● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2022-09-26 10:11:45 UTC; 5min ago
       Docs: https://httpd.apache.org/docs/2.4/
   Main PID: 2555 (apache2)
      Tasks: 55 (limit: 1143)
     Memory: 4.8M
        CPU: 43ms
     CGroup: /system.slice/apache2.service
             ├─2555 /usr/sbin/apache2 -k start
             ├─2557 /usr/sbin/apache2 -k start
             └─2558 /usr/sbin/apache2 -k start

Sep 26 10:11:45 ip-172-31-31-44 systemd[1]: Starting The Apache HTTP Server...
Sep 26 10:11:45 ip-172-31-31-44 systemd[1]: Started The Apache HTTP Server.
ubuntu@ip-172-31-31-44:~$

# Open TCP port 80 in AWS which is the default port that web browsers use to access web pages on the Internet

# Access the server in Ubuntu Shell
ubuntu@ip-172-31-31-44:~$ curl http://localhost:80

ubuntu@ip-172-31-31-44:~$ curl -s http://169.254.169.254/latest/meta-data/public-ipv4
18.222.228.181ubuntu@ip-172-31-31-44:~$

# Install MySQL Server and Setup Root Password
    ubuntu@ip-172-31-31-44:~$ sudo apt install mysql-server
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libcgi-fast-perl libcgi-pm-perl libclone-perl libencode-locale-perl libevent-pthreads-2.1-7
  libfcgi-bin libfcgi-perl libfcgi0ldbl libhtml-parser-perl libhtml-tagset-perl libhtml-template-perl
  libhttp-date-perl libhttp-message-perl libio-html-perl liblwp-mediatypes-perl libmecab2
  libprotobuf-lite23 libtimedate-perl liburi-perl mecab-ipadic mecab-ipadic-utf8 mecab-utils
  mysql-client-8.0 mysql-client-core-8.0 mysql-common mysql-server-8.0 mysql-server-core-8.0


# Log into MySQL-console
ubuntu@ip-172-31-31-44:~$ sudo mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.30-0ubuntu0.22.04.1 (Ubuntu)

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>

# Run a security script that comes pre-installed with MySQL
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
Query OK, 0 rows affected (0.01 sec)

mysql>

# Exit the MySQL shell
    mysql> exit
    Bye
    ubuntu@ip-172-31-31-44:~$

# MySQL secure Installation
    ubuntu@ip-172-31-31-44:~$ sudo mysql_secure_installation

Securing the MySQL server deployment.

Enter password for user root:

VALIDATE PASSWORD COMPONENT can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD component?

Press y|Y for Yes, any other key for No:
Using existing password for root.
Change the password for root ? ((Press y|Y for Yes, any other key for No) :

 ... skipping.
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
Success.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y
Success.

By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
 - Dropping test database...
Success.

 - Removing privileges on test database...
Success.

Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.

All done!
ubuntu@ip-172-31-31-44:~$

# Install PHP
    ubuntu@ip-172-31-31-44:~$ sudo apt install php libapache2-mod-php php-mysql
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libapache2-mod-php8.1 php-common php8.1 php8.1-cli php8.1-common php8.1-mysql php8.1-opcache
  php8.1-readline
Suggested packages:
  php-pear
The following NEW packages will be installed:
  libapache2-mod-php libapache2-mod-php8.1 php php-common php-mysql php8.1 php8.1-cli php8.1-common
  php8.1-mysql php8.1-opcache php8.1-readline
0 upgraded, 11 newly installed, 0 to remove and 38 not upgraded.
Need to get 5261 kB of archives.

# Create the directory for projectlamp using ‘mkdir’

ubuntu@ip-172-31-31-44:~$ sudo mkdir /var/www/projectlamp
ubuntu@ip-172-31-31-44:~$ cd /var/www
ubuntu@ip-172-31-31-44:/var/www$ ls -ltr
total 8
drwxr-xr-x 2 root root 4096 Sep 26 10:11 html
drwxr-xr-x 2 root root 4096 Sep 26 11:44 projectlamp
ubuntu@ip-172-31-31-44:/var/www$

# Change user ownerships
ubuntu@ip-172-31-31-44:/var/www/projectlamp$ sudo chown -R $USER:$USER /var/www/projectlamp
ubuntu@ip-172-31-31-44:/var/www/projectlamp$

# Create Virtual Host conf for project
    sudo vim /etc/apache2/sites-available/projectlamp.conf

    <VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

# show the new file in the sites-available directory
root@ip-172-31-31-44:/var/www# sudo ls /etc/apache2/sites-available
000-default.conf  default-ssl.conf  projectlamp.conf
root@ip-172-31-31-44:/var/www#

# Enable the new virtual host
root@ip-172-31-31-44:/var/www# sudo a2ensite projectlamp
Enabling site projectlamp.
To activate the new configuration, you need to run:
  systemctl reload apache2
root@ip-172-31-31-44:/var/www#

# Disable default site
root@ip-172-31-31-44:/# sudo a2dissite 000-default
Site 000-default disabled.
To activate the new configuration, you need to run:
  systemctl reload apache2
root@ip-172-31-31-44:/#

# Test Configuration
root@ip-172-31-31-44:/# sudo apache2ctl configtest
Syntax OK
root@ip-172-31-31-44:/#

# Reload Apache2
root@ip-172-31-31-44:/# sudo systemctl reload apache2
root@ip-172-31-31-44:/#

# Create index.html file under /var/www/projectlamp
  root@ip-172-31-31-44:/# sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
root@ip-172-31-31-44:/#

# Edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed 
   root@ip-172-31-31-44:/# sudo vim /etc/apache2/mods-enabled/dir.conf

    <IfModule mod_dir.c>
DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>

# Create a new file named index.php inside your custom web root folder
root@ip-172-31-31-44:/# vim /var/www/projectlamp/index.php
root@ip-172-31-31-44:/#

root@ip-172-31-31-44:/# cat /var/www/projectlamp/index.php
<?php
phpinfo();
root@ip-172-31-31-44:/#






