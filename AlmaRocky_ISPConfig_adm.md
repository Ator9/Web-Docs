SSH Key + mRemoteNG: https://technotes.khitrenovich.com/opening-ssh-aws-hosted-linux-servers-mremoteng/

## Rocky Linux 9 - ISPConfig Multiserver - Admin & HTTP

# 1. Startup
```sh
my_db_pass=XXX

my_adm_host=XXX
my_adm_ip=XXX
my_http_host=XXX
my_http_ip=XXX
```

```sh
yum update -y
yum install -y nano git telnet nmap quota epel-release langpacks-en
yum install -y yum-utils clamav clamd clamav-update rkhunter

ln -sf /usr/share/zoneinfo/America/Argentina/Buenos_Aires /etc/localtime
sed -i -e 's/PermitRootLogin yes/PermitRootLogin without-password/' /etc/ssh/sshd_config
service sshd restart

echo "$my_adm_ip     $my_adm_host" >> /etc/hosts
echo "$my_http_ip     $my_http_host" >> /etc/hosts
```

# 2. Swap, quota, firewall
```sh
sudo fallocate -l 1G /var/swap.img ; chmod 600 /var/swap.img
mkswap /var/swap.img ; swapon /var/swap.img
echo "/var/swap.img    none    swap    sw    0    0" >> /etc/fstab

mount | grep ' / '
sed -i '0,/console=tty0/s//console=tty0 rootflags=uquota,gquota/' /etc/default/grub
grub2-mkconfig -o /boot/grub2/grub.cfg

systemctl stop firewalld.service ; systemctl disable firewalld.service
sed -i -e "s/=enforcing/=permissive/g" /etc/selinux/config

reboot
```

# 3. MariaDB & GRANT access to servers
```sh
yum install -y mariadb-server
systemctl start mariadb.service ; systemctl enable mariadb.service
/usr/bin/mysql_secure_installation

sed -i -e "s/\[mysqld\]/\[mysqld\]\nbind-address = $my_adm_ip/g" /etc/my.cnf.d/mariadb-server.cnf
service mariadb restart

```
GRANT access to servers:
```sh
mysql -uroot -p$my_db_pass -e "DROP USER 'root'@'$my_adm_host';FLUSH PRIVILEGES"

mysql -uroot -p$my_db_pass -e "CREATE USER 'root'@'$my_adm_host' IDENTIFIED BY '$my_db_pass'"
mysql -uroot -p$my_db_pass -e "GRANT ALL PRIVILEGES ON *.* TO  'root'@'$my_adm_host' IDENTIFIED BY '$my_db_pass' WITH GRANT OPTION MAX_QUERIES_PER_HOUR 0 MAX_CONNECTIONS_PER_HOUR 0 MAX_UPDATES_PER_HOUR 0 MAX_USER_CONNECTIONS 0"

mysql -uroot -p$my_db_pass -e "CREATE USER 'root'@'$my_http_host' IDENTIFIED BY '$my_db_pass'"
mysql -uroot -p$my_db_pass -e "GRANT ALL PRIVILEGES ON *.* TO  'root'@'$my_http_host' IDENTIFIED BY '$my_db_pass' WITH GRANT OPTION MAX_QUERIES_PER_HOUR 0 MAX_CONNECTIONS_PER_HOUR 0 MAX_UPDATES_PER_HOUR 0 MAX_USER_CONNECTIONS 0"

mysql -uroot -p$my_db_pass -e "SHOW DATABASES;SELECT User,Host FROM mysql.user"
```

# 4. Apache & PHP
```sh
yum install -y httpd mod_ssl
service httpd start ; systemctl enable httpd.service
yum install -y php php-devel php-gd php-ldap php-odbc php-pear php-xml php-mbstring php-snmp php-soap php-tidy curl curl-devel perl-libwww-perl ImageMagick libxml2 libxml2-devel php-cli httpd-devel unzip bzip2 perl-DBD-mysql php-fpm mod_fcgid php_mysqli
systemctl start php-fpm.service ; systemctl enable php-fpm.service

echo "RequestHeader unset Proxy early" >> /etc/httpd/conf/httpd.conf
echo "AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css text/javascript application/javascript" >> /etc/httpd/conf/httpd.conf
echo 'ServerTokens Prod' >> /etc/httpd/conf/httpd.conf

sed -i -e 's/short_open_tag = Off/short_open_tag = On/g' /etc/php.ini
sed -i -e 's/expose_php = On/expose_php = Off/g' /etc/php.ini

service httpd restart

```

# 5. ISPConfig
### Install
```sh
wget http://www.ispconfig.org/downloads/ISPConfig-3-stable.tar.gz
tar -zxvf ISPConfig-3-stable.tar.gz
sudo php -q ispconfig3_install/install/install.php
```
- Master Server: Expert mode ("N" to mail/jailkit/pureftpd/dns/OpenVZ/firewall, "Y" rest)
- Web Server: Expert mode ("N" to mail/dns/OpenVZ, "Y" rest)

### Updates
```sh
ispconfig_update.sh
/usr/local/ispconfig/server/scripts/ispconfig_update.sh
```
- Method: stable
- Reconfigure Permissions: yes
- Reconfigure Services: yes
- Reconfigure Crontab: yes


## A. phpMyAdmin (Master Server)
```sh
yum install -y phpmyadmin
sed -i -e 's/Require local/Require local\nRequire all granted/' /etc/httpd/conf.d/phpMyAdmin.conf
echo "\$cfg['Servers'][\$i]['hide_db'] = '^information_schema|dbispconfig|performance_schema|mysql\$';" >> /etc/phpMyAdmin/config.inc.php
service httpd restart
```
```sh
nano /etc/phpMyAdmin/config.inc.php
```
```php
if(!in_array($_SERVER['REMOTE_ADDR'], array('yourip'))) exit();
```

## B. PureFTPd & Jailkit (Web Server)
```sh
yum install -y pure-ftpd gcc
systemctl enable pure-ftpd.service; systemctl start pure-ftpd.service
wget http://olivier.sessink.nl/jailkit/jailkit-2.23.tar.gz
tar -zxvf jailkit-2.23.tar.gz
cd jailkit-2.23 ; ./configure
make ; sudo make install
cd .. ; rm -rf jailkit-2.23*
```

Secure PureFTPd (Optional)
```sh
mkdir -p /etc/ssl/private/
openssl req -x509 -nodes -days 7300 -newkey rsa:2048 -keyout /etc/ssl/private/pure-ftpd.pem -out /etc/ssl/private/pure-ftpd.pem

```
```sh
chmod 600 /etc/ssl/private/pure-ftpd.pem
echo "TLS    2" >> /etc/pure-ftpd/pure-ftpd.conf
systemctl restart pure-ftpd.service
```

# Final
```sh
echo > ~/.bash_history ; history -c
```
