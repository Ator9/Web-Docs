# Debian 12 - ISPConfig Multiserver - Admin & HTTP

Debug: https://www.faqforge.com/linux/debugging-ispconfig-3-server-actions-in-case-of-a-failure/

```sh
sudo su -
systemctl reboot
systemctl restart apache2
systemctl restart php8.3-fpm
systemctl restart supervisor (apt install supervisor | nano /etc/supervisor/conf.d/laravel-worker.conf)
/etc/init.d/fail2ban restart

nano /etc/php/8.3/fpm/php.ini
nano /etc/apache2/apache2.conf

# Google DNS
# /etc/network/interfaces > dns-nameservers  8.8.8.8 8.8.4.4
systemctl status systemd-networkd.service
systemctl restart systemd-networkd.service

cat /var/log/ispconfig/acme.log
cat /var/log/ispconfig/cron.log
cat /var/log/supervisor/supervisord.log
cat /var/mail/root

cat /var/log/php8.3-fpm.log

tail -200 /var/log/apache2/access.log
tail -200 /var/log/apache2/error.log

echo > ~/.bash_history ; history -c
# delete single line:
history -d 1234 ; history -w
```

# 1. Startup & Swap
```sh
su - (this method needed, then "sudo su -" to enter faster)
passwd ; adduser XXX ; passwd XXX

apt update && apt upgrade -y

echo "127.0.1.1 server1.domain.com server1" >> /etc/hosts
systemctl reboot
```
Check if "prohibit-password" is default
```sh
sed -i -e 's/PermitRootLogin yes/PermitRootLogin prohibit-password/' /etc/ssh/sshd_config
systemctl restart ssh.service
```
```sh
ln -sf /usr/share/zoneinfo/America/Argentina/Buenos_Aires /etc/localtime

sudo swapon --show
sudo fallocate -l 2G /var/swap.img ; chmod 600 /var/swap.img
sudo mkswap /var/swap.img ; sudo swapon /var/swap.img
echo "/var/swap.img    none    swap    sw    0    0" >> /etc/fstab
```

# 2. ISPConfig Install
```sh
wget -O - https://get.ispconfig.org | sh -s -- --no-mail --no-dns --no-roundcube --use-php=8.1,8.2,8.3,8.4

echo "\$cfg['Servers'][\$i]['hide_db'] = '^information_schema|dbispconfig|performance_schema|mysql|phpmyadmin|sys\$';" >> /usr/share/phpmyadmin/config.inc.php
```

If debian deletes phpmyadmin:
```sh
sudo chmod -x /etc/cron.daily/auto_update_phpmyadmin
```

# 3. ISPConfig Update (Optional / If Needed) - <a href="http://www.faqforge.com/linux/controlpanels/ispconfig3/how-to-update-ispconfig-3/" target="_blank">Notes</a>
```sh
ispconfig_update.sh
/usr/local/ispconfig/server/scripts/ispconfig_update.sh
```

Master / Slaves Config:
- Method: stable
- Reconfigure Permissions: no
- Reconfigure Services: yes
- Reconfigure Crontab: yes

# 4.  Force BIND9 to use IPv4 Only (to avoid curl errors)
```sh
sudo nano /etc/default/named

OPTIONS="-u bind -4"

sudo systemctl restart bind9
```

# EXTRA
## SSH Key + mRemoteNG (Parameters > PPK file version 2). Delete old ppk file if issues.
https://technotes.khitrenovich.com/opening-ssh-aws-hosted-linux-servers-mremoteng/

# ISPConfig Ports (Azure Add 8080)
Web/FTP: 21,22,80,443,8080,8081 (TCP)

Mail: 25,110,143,465,587,993,995 (TCP)

Database: 22,3306 (TCP)
