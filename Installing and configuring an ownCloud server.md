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
```
sed -i "s#html#owncloud#" /etc/apache2/sites-available/000-default.conf
service apache2 restart
```

b) Create a virtual host configuration:
```
FILE="/etc/apache2/sites-available/owncloud.conf"
/bin/cat <<EOM >$FILE
Alias /owncloud "/var/www/owncloud/"
<Directory /var/www/owncloud/>
  Options +FollowSymlinks
  AllowOverride All
 <IfModule mod_dav.c>
  Dav off
 </IfModule>
 SetEnv HOME /var/www/owncloud
 SetEnv HTTP_HOME /var/www/owncloud
 </Directory>
EOM
```

c) Enable the virtual host configuration
```
a2ensite owncloud.conf
service apache2 reload
```

7. Configure the database:
```
service mysql start
mysql -u root -e "CREATE DATABASE IF NOT EXISTS owncloud; \
GRANT ALL PRIVILEGES ON owncloud.* \
TO owncloud@localhost \
IDENTIFIED BY 'password'";
```
Run the below command to enable the recommended Apache modules:
```
echo "Enabling Apache Modules"
a2enmod dir env headers mime rewrite setenvif
service apache2 reload
```
8. Download ownCloud:
```
cd /var/www/
wget https://download.owncloud.org/community/owncloud-10.8.0.tar.bz2 && \
tar -xjf owncloud-10.8.0.tar.bz2 && \
chown -R www-data. Owncloud
```
9. Install ownCloud:
```
occ maintenance:install \
    --database "mysql" \
    --database-name "owncloud" \
    --database-user "owncloud" \
    --database-pass "password" \
    --admin-user "admin" \
    --admin-pass "admin"
```
10. Configure the ownCloud’s trusted domains:
```
myip=$(hostname -I|cut -f1 -d ' ')
occ config:system:set trusted_domains 1 --value="$myip"
```
11. Run the below command to set your background job mode to cron:
```
occ background:cron
echo "*/15  *  *  *  * /var/www/owncloud/occ system:cron" \
  > /var/spool/cron/crontabs/www-data
chown www-data.crontab /var/spool/cron/crontabs/www-data
chmod 0600 /var/spool/cron/crontabs/www-data
```
12. Execute the below commands to configure caching and file locking:
```
occ config:system:set \
   memcache.local \
   --value '\OC\Memcache\APCu'
occ config:system:set \
   memcache.locking \
   --value '\OC\Memcache\Redis'
service redis-server start
occ config:system:set \
   redis \
   --value '{"host": "127.0.0.1", "port": "6379"}' \
   --type json
```
13. Run the below command to configure log rotation:
```
FILE="/etc/logrotate.d/owncloud"
sudo /bin/cat <<EOM >$FILE
/var/www/owncloud/data/owncloud.log {
  size 10M
  rotate 12
  copytruncate
  missingok
  compress
  compresscmd /bin/gzip
}
EOM
```
14. Complete the installation and check the access permissions are correct using the below command:
 ```
 cd /var/www/
 chown -R www-data. owncloud
```
15. For verifying that the installation is successful, perform these steps:
a) Type the URL of the ownCloud server in your browser’s address bar. The ownCloud login window appears.
b) Type your username and password.
c) Click the **Log in** button. The ownCloud main interface appears.
