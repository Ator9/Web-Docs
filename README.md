Commands
========
```sh
/usr/local/ispconfig/server/server.sh

service mariadb restart
service httpd restart
service pure-ftpd restart
service postfix restart

echo > ~/.bash_history ; history -c

sudo service rackspace-monitoring-agent restart

mysql -V
httpd -v
php -v
cat /etc/*release*

nano /etc/my.cnf
nano /etc/httpd/conf/httpd.conf
nano /etc/php.ini
nano /etc/postfix/main.cf

tail -100 /var/log/mysqld.log
tail -100 /var/log/httpd/error_log

scp file.tar.gz user@domain.net:/var/test

echo "\$i++;\$cfg['Servers'][\$i]['host'] = server.com';" >> /etc/phpMyAdmin/config.inc.php

nano /etc/sysconfig/network

killall nautilus

chown bob:group2 filedir

```
show top 10 biggest subdirs in the current dir

```sh
du -sk * | sort -nr | head -10
```

Bind
```sh
/var/www/clients/client1/web4/web /var/www/clients/client1/web5/web    none    bind,nobootwait,_netdev    0 0
mount -a
```

Mount
```sh
freenas.domain.net:/mnt/folder /home/myuser/freenas nfs defaults 0 0
mount -a
```

Comprimir tar.gz
```sh
tar -zcvf backup_2012.tar.gz directory-name

-z: Compress archive using gzip program
-c: Create archive
-v: Verbose i.e display progress while creating archive
-f: Archive File name
```
Descomprimir tar.gz
```sh
tar -zxvf  backup_2012.tar.gz
```

Flush All Rules, Delete All Chains, and Accept All
```sh
sudo iptables -P INPUT ACCEPT ; sudo iptables -P FORWARD ACCEPT ; sudo iptables -P OUTPUT ACCEPT

sudo iptables -t nat -F ; sudo iptables -t mangle -F ; sudo iptables -F ; sudo iptables -X ;
/etc/init.d/iptables save

csf -x
```
You can disable the "bandmin" cron jobs using the "crontab -e" command, as it's those cron jobs that add the iptables rules you are referring to. 
