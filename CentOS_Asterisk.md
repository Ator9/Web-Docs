CentOS 7 - Asterisk
```sh
my_ssh_user=XXX
```
#1. Startup & fail2ban
```sh
history -c
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

#2. Asterisk Dependencies
```sh
yum -y groupinstall core base
yum install -y gcc gcc-c++ lynx bison mariadb-devel mariadb-server gmime-devel psmisc php php-mysql php-pear php-mbstring tftp-server httpd make ncurses-devel libtermcap-devel sendmail sendmail-cf caching-nameserver sox newt-devel libxml2-devel libtiff-devel audiofile-devel gtk2-devel subversion kernel-devel git subversion kernel-devel php-process crontabs cronie cronie-anacron wget vim php-xml uuid-devel libtool sqlite-devel
service httpd start ; systemctl enable httpd.service
mkdir /var/www/html ; chmod 777 /var/www/html
systemctl start mariadb.service ; systemctl enable mariadb.service
pear install db-1.7.14
systemctl stop firewalld.service ; systemctl disable firewalld.service ; iptables -L -n
sed -i -e 's/SELINUX=permissive/SELINUX=disabled/g' /etc/selinux/config ; sestatus
reboot

```

#3. Asterisk
```sh
adduser asterisk -M -c "asterisk"
cd /usr/src
wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-13-current.tar.gz
git clone https://github.com/akheron/jansson.git
wget http://www.pjsip.org/release/2.2.1/pjproject-2.2.1.tar.bz2

```
