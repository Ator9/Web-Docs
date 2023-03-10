Commands
========
```sh
/usr/local/ispconfig/server/server.sh

service mariadb restart
service httpd restart
service pure-ftpd restart
service postfix restart

echo > ~/.bash_history ; history -c

cat /var/log/secure | grep "Accepted password"

* * * * *   root    pgrep httpd > /dev/null || /bin/systemctl start httpd.service
* * * * *   root    pgrep mysqld > /dev/null || /bin/systemctl start mariadb.service

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
tail -100 /var/log/mariadb/mariadb.log

scp file.tar.gz user@domain.net:/home/user
scp -r user@your.server.example.com:/path/to/foo /home/user/Desktop/

bash-4.2# sudo /sbin/reboot --force

cd /opt/certbot/
./certbot-auto renew --force-renewal
sudo certbot delete
```

Nameservers NIC.ar
```sh
whois -h whois.nic.ar DOMINIO.COM.AR

```

ISPCONFIG /usr/local/ispconfig/server/plugins-available/webserver_plugin.inc.php
```sh
if(!function_exists("array_column")){
        function array_column($array,$column_name){
                return array_map(function($element) use($column_name){return $element[$column_name];}, $array);
        }
}

```


Clean CentOS Cache
```sh
yum clean all
rm -rf /var/cache/yum/

# Remove orphan packages
package-cleanup --quiet --leaves --exclude-bin
package-cleanup --quiet --leaves --exclude-bin | xargs yum remove

# Remove old kernels
package-cleanup --oldkernels --count=2
```

External Public IP
```sh
dig +short myip.opendns.com @resolver1.opendns.com
```

How do I force Git to overwrite local files on pull?
```sh
error: Your local changes to the following files would be overwritten by merge:
error: The following untracked working tree files would be overwritten by merge:

git fetch --all
git reset --hard origin/master
```

Check HTTPS Certificate. Check invalid redirects.
```sh
curl -IvL https://domain.me
```

Email
```sh
mail -s "asunto ejemplo" your@email.com <<< "mensaje"
```

show top 10 biggest subdirs in the current dir

```sh
du -sk * | sort -nr | head -10

df -h
```

Find files with specific size (bytes)
```sh
find /var/www/web.me/web/ -type f -size 85796c -exec ls -lh {} \;
```
```sh
find /var/www/web.me/web/ -type f -size +85550c -size -86000c -exec ls -lh {} \;
```

Find files with specific text within
```sh
grep -rnw '/path/to/somewhere/' -e 'pattern'
```

Bind
```sh
/var/www/clients/client1/web4/web /var/www/clients/client1/web5/web    none    bind,nobootwait,_netdev    0 0
mount -a
```

Delete queued mail
```sh
postqueue -p
postsuper -d ALL
```

Comprimir tar.gz
```sh
tar -zcvf backup_2012.tar.gz directory-name
```

Descomprimir tar.gz
```sh
tar -zxvf  backup_2012.tar.gz
```

Mount wp-content in another disk (ispconfig PHP open_basedir :/data)
```sh
cp -fr --preserve wp-content /data/wp-content
mv wp-content wp-content.old

ln -sf /data/uploads uploads
chown -h web7:client1 uploads
```

Search repo rpm and delete (to solve yum conflicts)
```sh
rpm -qa | grep -i repo-name
rpm -e repo-name
```

Compsoer 2 - Centos 7
```sh
cd /usr/bin
https://getcomposer.org/download/
sudo mv composer.phar /usr/bin/composer
```
sudo -u web135 composer install
