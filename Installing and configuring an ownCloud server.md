# Welcome to Shipra's Repository
ownClod Quickstart help

[Connecting to the ownCloud server](/Connecting to the ownCloud server.md)

[Installing and configuring an ownCloud server](/Installing and configuring an ownCloud server.md)

[Creating a User account](/Creating a user account.md)

[Connect to ownCloud using port](/Connect to ownCloud using port.md)

ownCloud is a SAAS-based solution that provides a safe approach for file synchronization and sharing in real time. You can easily share files and folders on your local machine with the ownCloud server deployed and synchronize those files if you make any changes later. 
You can deploy an ownCloud server on-premises or off-premises depending on your requirements.
# Prerequisites
For a standard ownCloud server installation, ensure these prerequisites are in place: 
* Operating system- Linux (Ubuntu 18.04 or Ubuntu 20.04 with SSH enabled)
* Web server Apache 2.4
* Database: MySQL/MariaDB with InnoDB storage engine
* A latest PHP version 

# Recommendation
 * To allow ownCloud administrators to have access to both command-line and cron
 * The ownCloud administrator needs to connect as the root user

# To install and configure ownCloud on Ubuntu
1. Run the below command to check all the installed packages are updated and PHP is available in the Advanced Package Tool (APT) repository:
```
apt update && \
apt upgrade -y
```
2. Create a helper script for executing occ commands:
```
FILE="/usr/local/bin/occ"
/bin/cat <<EOM >$FILE
#! /bin/bash
cd /var/www/owncloud
sudo -u www-data /usr/bin/php /var/www/owncloud/occ "\$@"
EOM
```
Where, _/var/www/owncloud_ is the ownCloud directory.
3. Use the below command to make the helper script executable:
```
chmod +x /usr/local/bin/occ
```
4. Use the below commands to install the required packages:
```
apt install -y \
  apache2 \
  libapache2-mod-php \
  mariadb-server \
  openssl \
  php-imagick php-common php-curl \
  php-gd php-imap php-intl \
  php-json php-mbstring php-mysql \
  php-ssh2 php-xml php-zip \
  php-apcu php-redis redis-server \
  wget
```
5. Use the below commands to install the recommended packages:
```
apt install -y \
  ssh bzip2 sudo cron rsync curl jq \
  inetutils-ping smbclient php-libsmbclient \
  php-smbclient coreutils php-ldap
```
6. Configure Apache:

a) Change the document root:
b) Create a virtual host configuration:
c) Enable the virtual host configuration
7. Configure the database:
Run the below command to enable the recommended Apache modules:
8. Download ownCloud:
9. Install ownCloud:
10. Configure the ownCloud’s trusted domains:
11. Run the below command to set your background job mode to cron:
12. Execute the below commands to configure caching and file locking:
13. Run the below command to configure log rotation:
14. Complete the installation and check the access permissions are correct using the below command:
15. For verifying that the installation is successful, perform these steps:

a) Type the URL of the ownCloud server in your browser’s address bar. The ownCloud login window appears.
b) Type your username and password.
c) Click the **Log in** button. The ownCloud main interface appears.
