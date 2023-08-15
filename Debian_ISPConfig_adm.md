# Debian 12 - ISPConfig Multiserver - Admin & HTTP

Debug: https://www.faqforge.com/linux/debugging-ispconfig-3-server-actions-in-case-of-a-failure/

```sh
su -
systemctl reboot

cat /var/log/ispconfig/cron.log
cat /var/log/ispconfig/acme.log

tail -200 /var/log/apache2/access_log
tail -200 /var/log/apache2/error.log

echo > ~/.bash_history ; history -c
```

```sh
my_db_pass=xxx
my_adm_host=XXX
my_adm_ip=XXX
my_http_host=XXX
my_http_ip=XXX

echo "$my_adm_ip     $my_adm_host" >> /etc/hosts
echo "$my_http_ip     $my_http_host" >> /etc/hosts
```

# 1. Startup & Swap
```sh
passwd ; adduser XXX ; passwd XXX

apt update && apt upgrade -y

ln -sf /usr/share/zoneinfo/America/Argentina/Buenos_Aires /etc/localtime
sed -i -e 's/PermitRootLogin yes/PermitRootLogin without-password/' /etc/ssh/sshd_config
systemctl restart ssh.service
sudo swapon --show
sudo fallocate -l 2G /var/swap.img ; chmod 600 /var/swap.img
sudo mkswap /var/swap.img ; sudo swapon /var/swap.img
echo "/var/swap.img    none    swap    sw    0    0" >> /etc/fstab
```

# 2. ISPConfig
Log in as root or switch using su -
```sh
wget -O - https://get.ispconfig.org | sh -s -- --no-mail --no-dns --no-roundcube --use-php=8.2,8.3
```

# 3. MariaDB & GRANT access to servers (multiserver only)
Set server private ip
```sh
sed -i -e "s/\[mysqld\]/\[mysqld\]\nbind-address = $my_adm_ip/g" /etc/mysql/mariadb.conf.d/50-server.cnf
service mariadb restart
```
GRANT access to servers:
```sh
mysql -uroot -p$my_db_pass -e "DROP USER 'root'@'$my_adm_host';FLUSH PRIVILEGES"
mysql -uroot -p$my_db_pass -e "DROP USER 'root'@'$my_http_host';FLUSH PRIVILEGES"

mysql -uroot -p$my_db_pass -e "CREATE USER 'root'@'$my_adm_host' IDENTIFIED BY '$my_db_pass'"
mysql -uroot -p$my_db_pass -e "GRANT ALL PRIVILEGES ON *.* TO  'root'@'$my_adm_host' IDENTIFIED BY '$my_db_pass' WITH GRANT OPTION MAX_QUERIES_PER_HOUR 0 MAX_CONNECTIONS_PER_HOUR 0 MAX_UPDATES_PER_HOUR 0 MAX_USER_CONNECTIONS 0"

mysql -uroot -p$my_db_pass -e "CREATE USER 'root'@'$my_http_host' IDENTIFIED BY '$my_db_pass'"
mysql -uroot -p$my_db_pass -e "GRANT ALL PRIVILEGES ON *.* TO  'root'@'$my_http_host' IDENTIFIED BY '$my_db_pass' WITH GRANT OPTION MAX_QUERIES_PER_HOUR 0 MAX_CONNECTIONS_PER_HOUR 0 MAX_UPDATES_PER_HOUR 0 MAX_USER_CONNECTIONS 0"

mysql -uroot -p$my_db_pass -e "SHOW DATABASES;SELECT User,Host FROM mysql.user"
```

# 4. Apache & PHP
```sh
echo "RequestHeader unset Proxy early" >> /etc/httpd/conf/httpd.conf 
echo "AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css text/javascript application/javascript" >> /etc/httpd/conf/httpd.conf
echo 'ServerTokens Prod' >> /etc/httpd/conf/httpd.conf

sed -i -e 's/;error_log = php_errors.log/error_log = \/var\/log\/php_errors.log/g' /etc/php/8.2/fpm/php.ini

systemctl restart apache2

```

# 5. phpMyAdmin (Master Server only)
```sh
echo "\$cfg['Servers'][\$i]['hide_db'] = '^information_schema|dbispconfig|performance_schema|mysql|phpmyadmin|sys\$';" >> /usr/share/phpmyadmin/config.inc.php
echo "if("'!'"in_array(\$_SERVER['REMOTE_ADDR'], array('yourip'))) exit();" >> /usr/share/phpmyadmin/config.inc.php
```
```sh
nano /usr/share/phpmyadmin/config.inc.php
```

# EXTRA
## SSH Key + mRemoteNG (Parameters > PPK file version 2). Delete old ppk file if issues.
https://technotes.khitrenovich.com/opening-ssh-aws-hosted-linux-servers-mremoteng/
