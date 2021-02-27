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

sudo service rackspace-monitoring-agent restart

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

cd /opt/certbot/
./certbot-auto renew --force-renewal
```

Nameservers NIC.ar
```sh
whois -h whois.nic.ar DOMINIO.COM.AR

```

Clean CentOS Cache
```sh
yum clean all
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

PfSense Block Ips/Domains
```sh
http://www.serdarbayram.net/blocking-https-facebook-and-twitter-on-pfsense.html

whois -h whois.radb.net -- '-i origin AS32934' | grep ^route
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

Mount
```sh
freenas.domain.net:/mnt/folder /home/myuser/freenas nfs defaults 0 0
mount -a
```

Network digitalocean config example
```sh
/etc/sysconfig/network-scripts/ifcfg-eth0
service network restart
```
```sh
BOOTPROTO=none
DEVICE=eth0
HWADDR=x
ONBOOT=yes
TYPE=Ethernet
USERCTL=no
GATEWAY=x
IPADDR=x
NETMASK=x
DNS1=8.8.8.8
DNS2=8.8.4.4
NM_CONTROLLED="YES"
```

Network reset
http://api.curltools.com/scripts/resetnetwork
```sh
nano netreset.sh
```
```sh
#!/bin/bash

#how to reset your network from inside your server#

#generate uuid#
uuid=$(uuidgen)

#grab networking data from host sever#
xenstore-write data/host/$uuid '{"name":"resetnetwork","value":""}'

secs=$((1 * 60))
while [ $secs -gt 0 ]; do
   echo -ne "$secs\033[0K\r"
   sleep 1
   : $((secs--))
done

#confirm that the reset took place#
xenstore-read data/guest/$uuid
#you should get a value of 0 in the responce to this command. Anything else means something is still wrong with nova-agent.#
```
```sh
chmod +x netreset.sh
./netreset.sh
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

Mount wp-content in another disk
```sh
cp -fr --preserve wp-content /data/wp-content
mv wp-content wp-content.old

ln -sf /data/uploads uploads
```

Asterisk Restart from CLI Admin
```sh
core restart now
```

Search repo rpm and delete (to solve yum conflicts)
```sh
rpm -qa | grep -i repo-name
rpm -e repo-name
```

Flush All Rules, Delete All Chains, and Accept All
```sh
sudo iptables -P INPUT ACCEPT ; sudo iptables -P FORWARD ACCEPT ; sudo iptables -P OUTPUT ACCEPT

sudo iptables -t nat -F ; sudo iptables -t mangle -F ; sudo iptables -F ; sudo iptables -X ;
/etc/init.d/iptables save

csf -x
```
You can disable the "bandmin" cron jobs using the "crontab -e" command, as it's those cron jobs that add the iptables rules you are referring to. 
