Commands
========
```sh
/usr/local/ispconfig/server/server.sh

service mariadb restart
service httpd restart
service pure-ftpd restart
service postfix restart

git checkout -- file.xxx

echo > ~/.bash_history ; history -c
# delete single line:
history -d 1234 ; history -w

cat /var/log/secure | grep "Accepted password"

* * * * *   root    pgrep httpd > /dev/null || /bin/systemctl start httpd.service
* * * * *   root    pgrep mariadb > /dev/null || /bin/systemctl start mariadb.service
* * * * *   root    pgrep supervisor > /dev/null || /bin/systemctl start supervisor
0 4 * * *   root    /bin/systemctl restart mariadb.service

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
scp -r plugins user@domain.net:/home/user

scp -r user@your.server.example.com:/path/to/foo /home/user/Desktop/

rsync -ruvt --delete folder user@222.222.222.222:/data

bash-4.2# sudo /sbin/reboot --force
```

FIX BITBUCKET
```sh
sudo -u web115 git pull
sudo echo > /var/www/site.com/.ssh/known_hosts
sudo -u web115 ssh-keygen -R bitbucket.org && curl https://bitbucket.org/site/ssh >> /var/www/site.com/.ssh/known_hosts
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

How do I force Git to overwrite local files on pull?
```sh
error: Your local changes to the following files would be overwritten by merge:
error: The following untracked working tree files would be overwritten by merge:

git fetch --all
git reset --hard origin/master
```

Reset file changes
```sh
git checkout -- file.xxx
```

Check HTTPS Certificate. Check invalid redirects.
```sh
curl -IvL https://domain.me
```

Email
```sh
mail -s "asunto ejemplo" your@email.com <<< "mensaje"
```

Delete queued mail
```sh
postqueue -p
postsuper -d ALL
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

Bind /etc/fstab (web/wordpress/ispconfig - systemctl daemon-reload if reload needed)
```sh
/var/www/clients/client1/web1/web /var/www/clients/client1/web2/web    none    bind,nobootwait,_netdev    0 0
mount -a
```

Comprimir tar.gz
```sh
tar -zcvf backup_2012.tar.gz directory-name
```

Descomprimir tar.gz
```sh
tar -zxvf  backup_2012.tar.gz
```

Search repo rpm and delete (to solve yum conflicts)
```sh
rpm -qa | grep -i repo-name
rpm -e repo-name
```

Composer 2 - Centos 7
```sh
cd /usr/bin
https://getcomposer.org/download/
sudo mv composer.phar /usr/bin/composer
```
sudo -u web135 composer install
