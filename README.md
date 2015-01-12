Commands
========
```sh
/usr/local/ispconfig/server/server.sh

service mariadb restart
service httpd restart
service pure-ftpd restart
service postfix restart

mysql -V
httpd -v
php -v

nano /etc/my.cnf
nano /etc/httpd/conf/httpd.conf
nano /etc/php.ini
nano /etc/postfix/main.cf

tail -100 /var/log/mysqld.log
tail -100 /var/log/httpd/error_log

scp file.tar.gz user@domain.net:/var/test

echo "\$i++;\$cfg['Servers'][\$i]['host'] = server.com';" >> /etc/phpMyAdmin/config.inc.php

```
show top 10 biggest subdirs in the current dir

```sh
du -sk * | sort -nr | head -10
```
