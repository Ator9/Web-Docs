CentOS 7.1 - Asterisk
```sh
my_ssh_user=XXX
my_db_pass=XXX
```
#1. Startup & fail2ban
```sh
echo > ~/.bash_history ; history -c
yum update -y
yum install -y ntp epel-release fail2ban
yum install -y fail2ban
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sed -i -e 's/bantime  = 600/bantime  = 3600/g' /etc/fail2ban/jail.local
systemctl enable fail2ban.service ; systemctl start fail2ban.service
sudo fallocate -l 1G /var/swap.img ; chmod 600 /var/swap.img
mkswap /var/swap.img ; swapon /var/swap.img
echo "/var/swap.img    none    swap    sw    0    0" >> /etc/fstab
ln -sf /usr/share/zoneinfo/America/Argentina/Buenos_Aires /etc/localtime
sed -i -e 's/#PermitRootLogin yes/PermitRootLogin without-password/' /etc/ssh/sshd_config
service sshd restart
adduser $my_ssh_user ; passwd $my_ssh_user
```

#2. System Setup
```sh
yum -y groupinstall core base
yum install -y gcc gcc-c++ lynx bison mariadb-devel mariadb-server gmime-devel psmisc php php-mysql php-pear php-mbstring tftp-server httpd make ncurses-devel libtermcap-devel sendmail sendmail-cf caching-nameserver sox newt-devel libxml2-devel libtiff-devel audiofile-devel gtk2-devel subversion kernel-devel git subversion kernel-devel php-process crontabs cronie cronie-anacron wget vim php-xml uuid-devel libtool sqlite-devel
service httpd start ; systemctl enable httpd.service
mkdir /var/www/html ; chmod 777 /var/www/html
systemctl start mariadb.service ; systemctl enable mariadb.service
pear install db-1.7.14
systemctl stop firewalld.service ; systemctl disable firewalld.service ; iptables -L -n
sed -i -e 's/SELINUX=permissive/SELINUX=disabled/g' /etc/selinux/config ; sestatus
/usr/bin/mysql_secure_installation
reboot

```

#3. Asterisk Dependencies
```sh
cd /usr/src
git clone https://github.com/akheron/jansson.git
cd jansson
autoreconf -i
./configure --libdir=/usr/lib64
make && make install

```


#4. Asterisk
```sh
adduser asterisk -M -c "Asterisk User"
cd /usr/src
wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-13-current.tar.gz
tar -zvxf asterisk-13-current.tar.gz
cd asterisk-*
contrib/scripts/install_prereq install
./configure --libdir=/usr/lib64
contrib/scripts/get_mp3_source.sh
make menuselect 
```
SELECT format_mp3 on this screen, then Save + Exit
```sh
make && make install && make config && ldconfig
chown -R asterisk.asterisk /var/run/asterisk
chown -R asterisk.asterisk /etc/asterisk
chown -R asterisk.asterisk /var/{lib,log,spool}/asterisk
chown -R asterisk.asterisk /usr/lib64/asterisk
chown -R asterisk.asterisk /var/www
make samples

```

#5. FreePBX
```sh
mysqladmin -uroot -p$my_db_pass create asterisk
mysqladmin -uroot -p$my_db_pass create asteriskcdrdb
mysql -uroot -p$my_db_pass -e "GRANT ALL PRIVILEGES ON asterisk.* TO asterisk@localhost IDENTIFIED BY '$my_db_pass';"
mysql -uroot -p$my_db_pass -e "GRANT ALL PRIVILEGES ON asteriskcdrdb.* TO asterisk@localhost IDENTIFIED BY '$my_db_pass';"
mysql -uroot -p$my_db_pass -e "flush privileges;"
```
```sh
cd /usr/src
wget http://mirror.freepbx.org/freepbx-12.0.3.tgz
tar -zvxf freepbx-12.0.3.tgz
cd freepbx
./start_asterisk start
rm -Rf /var/www/html/
./install_amp --installdb --username=asterisk --password=$my_db_pass
```
Config
```sh
ln -s /var/lib/asterisk/moh /var/lib/asterisk/mohmp3
./start_asterisk restart
amportal chown
amportal a ma installall
amportal a reload
amportal a ma refreshsignatures
amportal chown
amportal restart

sed -i 's/^\(User\|Group\).*/\1 asterisk/' /etc/httpd/conf/httpd.conf
sed -i -e 's/AllowOverride None/AllowOverride All/g' /etc/httpd/conf/httpd.conf
/bin/systemctl restart httpd.service
```

/etc/asterisk/manager.conf
