CentOS 7 - Multiserver - Admin
```sh
passwd ; adduser XXX ; passwd XXX
```
# 1. Startup
```sh
my_db_pass=XXX

my_adm_host=XXX
my_adm_ip=XXX
my_db_host=XXX
my_db_ip=XXX
my_http_host=XXX
my_http_ip=XXX
```
```sh
yum update -y
yum install -y telnet nmap quota ntp epel-release git yum-utils
yum install -y clamav clamav-update rkhunter
echo "$my_adm_ip     $my_adm_host" >> /etc/hosts
echo "$my_db_ip     $my_db_host" >> /etc/hosts
echo "$my_http_ip     $my_http_host" >> /etc/hosts
service network restart
ln -sf /usr/share/zoneinfo/America/Argentina/Buenos_Aires /etc/localtime
sed -i -e 's/#PermitRootLogin yes/PermitRootLogin without-password/' /etc/ssh/sshd_config
service sshd restart
chkconfig ip6tables off

```

# 2. Swap, quota, fail2ban & firewall
```sh
sudo fallocate -l 1G /var/swap.img ; chmod 600 /var/swap.img
mkswap /var/swap.img ; swapon /var/swap.img
echo "/var/swap.img    none    swap    sw    0    0" >> /etc/fstab
sed -i '0,/defaults/s//defaults,usrquota,grpquota/' /etc/fstab
mount -o remount /

quotacheck -avugm ; quotaon -avug
yum install -y fail2ban
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sed -i -e 's/bantime  = 600/bantime  = 3600/g' /etc/fail2ban/jail.local
systemctl enable fail2ban.service ; systemctl start fail2ban.service
systemctl stop firewalld.service ; systemctl disable firewalld.service
sed -i -e "s/=permissive/=disabled/g" /etc/selinux/config

```

# 3. MariaDB & GRANT access to servers
```sh
yum install -y mariadb-server
systemctl start mariadb.service ; systemctl enable mariadb.service
/usr/bin/mysql_secure_installation

```
GRANT access to servers:
```sh
mysql -uroot -p$my_db_pass -e "DROP USER 'root'@'$my_adm_host';FLUSH PRIVILEGES"

mysql -uroot -p$my_db_pass -e "CREATE USER 'root'@'$my_adm_host' IDENTIFIED BY '$my_db_pass'"
mysql -uroot -p$my_db_pass -e "GRANT ALL PRIVILEGES ON *.* TO  'root'@'$my_adm_host' IDENTIFIED BY '$my_db_pass' WITH GRANT OPTION MAX_QUERIES_PER_HOUR 0 MAX_CONNECTIONS_PER_HOUR 0 MAX_UPDATES_PER_HOUR 0 MAX_USER_CONNECTIONS 0"

mysql -uroot -p$my_db_pass -e "CREATE USER 'root'@'$my_db_host' IDENTIFIED BY '$my_db_pass'"
mysql -uroot -p$my_db_pass -e "GRANT ALL PRIVILEGES ON *.* TO  'root'@'$my_db_host' IDENTIFIED BY '$my_db_pass' WITH GRANT OPTION MAX_QUERIES_PER_HOUR 0 MAX_CONNECTIONS_PER_HOUR 0 MAX_UPDATES_PER_HOUR 0 MAX_USER_CONNECTIONS 0"

mysql -uroot -p$my_db_pass -e "CREATE USER 'root'@'$my_http_host' IDENTIFIED BY '$my_db_pass'"
mysql -uroot -p$my_db_pass -e "GRANT ALL PRIVILEGES ON *.* TO  'root'@'$my_http_host' IDENTIFIED BY '$my_db_pass' WITH GRANT OPTION MAX_QUERIES_PER_HOUR 0 MAX_CONNECTIONS_PER_HOUR 0 MAX_UPDATES_PER_HOUR 0 MAX_USER_CONNECTIONS 0"

mysql -uroot -p$my_db_pass -e "SHOW DATABASES;SELECT User,Host FROM mysql.user"
```

# 4. Apache & PHP
```sh
yum install -y httpd mod_ssl
service httpd start ; systemctl enable httpd.service
yum install -y php php-devel php-gd php-imap php-ldap php-mssql php-mysql php-odbc php-pear php-xml php-xmlrpc php-pecl-apc php-mbstring php-mcrypt php-snmp php-soap php-tidy curl curl-devel perl-libwww-perl ImageMagick libxml2 libxml2-devel php-cli httpd-devel unzip bzip2 perl-DBD-mysql php-fpm mod_fcgid
systemctl start php-fpm.service ; systemctl enable php-fpm.service
service httpd restart

```

# 5. phpMyAdmin
```sh
yum install -y phpmyadmin
sed -i -e 's/Require ip ::1/Require ip ::1\nRequire all granted/' /etc/httpd/conf.d/phpMyAdmin.conf
sed -i -e 's/?>//g' /etc/phpMyAdmin/config.inc.php
sed -i -e "s/localhost/$my_db_host/g" /etc/phpMyAdmin/config.inc.php
echo "\$cfg['Servers'][\$i]['hide_db'] = '^information_schema|dbispconfig|performance_schema|mysql\$';" >> /etc/phpMyAdmin/config.inc.php
service httpd restart

```

# 6. ISPConfig
Expert mode ("N" to mail/dns/interface, "Y" rest)
```sh
yum install -y awstats perl-DateTime-Format-HTTP perl-DateTime-Format-Builder perl-Time*
service httpd restart
wget http://www.ispconfig.org/downloads/ISPConfig-3-stable.tar.gz
tar -zxvf ISPConfig-3-stable.tar.gz
sudo php -q ispconfig3_install/install/install.php

```

# 7. Configuration
```sh
sed -i -e 's/short_open_tag = Off/short_open_tag = On/g' /etc/php.ini
sed -i -e 's/expose_php = On/expose_php = Off/g' /etc/php.ini
echo "AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css text/javascript application/javascript" >> /etc/httpd/conf/httpd.conf
echo 'KeepAlive On' >> /etc/httpd/conf/httpd.conf
echo 'KeepAliveTimeout 5' >> /etc/httpd/conf/httpd.conf
echo 'ServerTokens ProductOnly' >> /etc/httpd/conf/httpd.conf
echo 'ServerSignature Off' >> /etc/httpd/conf/httpd.conf
service httpd restart

```

# 8. Let's Encrypt (Optional / If Needed)
```sh
mkdir /opt/certbot
cd /opt/certbot
wget https://dl.eff.org/certbot-auto
chmod a+x ./certbot-auto
./certbot-auto
```

Remove OLD certbot-auto - <a href="https://certbot.eff.org/lets-encrypt/centosrhel7-apache" target="_blank">Notes</a> 
```sh
sudo rm -rf /opt/eff.org
```

Install NEW certbot
```sh
sudo yum install snapd
sudo systemctl enable --now snapd.socket
sudo ln -s /var/lib/snapd/snap /snap

sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```


# 9. ISPConfig Update (Optional / If Needed) - <a href="http://www.faqforge.com/linux/controlpanels/ispconfig3/how-to-update-ispconfig-3/" target="_blank">Notes</a>
```sh
ispconfig_update.sh
/usr/local/ispconfig/server/scripts/ispconfig_update.sh
```

Master / Slaves Config:
- Method: stable
- Reconfigure Permissions: yes
- Reconfigure Services: yes
- Reconfigure Crontab: yes


Update from 3.1 to 3.2 (CentOS 7):
```sh
sudo yum -y install ncurses-devel gcc geoip-devel tokyocabinet-devel lbzip2 p7zip xz-libs lzip
cd /tmp
wget http://tar.goaccess.io/goaccess-1.4.tar.gz
tar xfz goaccess-1.4.tar.gz
cd goaccess-1.4
sudo ./configure --enable-utf8 --enable-geoip=legacy
sudo make
sudo make install
sudo ln -s /usr/local/bin/goaccess /usr/bin/goaccess
```

# PHP 7.4
```sh
php --version
wget -q http://rpms.remirepo.net/enterprise/remi-release-7.rpm
rpm -Uvh remi-release-7.rpm 
yum-config-manager --enable remi-php74
yum install -y php php-pecl-zip ; yum update -y
sed -i -e 's/;error_log = php_errors.log/error_log = \/var\/log\/php_errors.log/g' /etc/php.ini
service httpd restart
php --version
```
```sh
yum-config-manager --disable remi-php71
yum repolist
```

# Automatic Domains
Paste before "NameVirtualHost" in /etc/httpd/conf/httpd.conf:
```sh
<VirtualHost serverip:80>
ServerAdmin mail@mail.com
DocumentRoot /var/www/domain.com/web
</VirtualHost>
<Directory /var/www/domain.com/web>
Require all granted
AllowOverride All
</Directory>
```
