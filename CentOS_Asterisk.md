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

#2. Asterisk
```sh
yum -y groupinstall core base
yum -y install automake gcc gcc-c++ ncurses-devel openssl-devel libxml2-devel unixODBC-devel libcurl-devel libogg-devel libvorbis-devel speex-devel spandsp-devel freetds-devel net-snmp-devel iksemel-devel corosynclib-devel newt-devel popt-devel libtool-ltdl-devel lua-devel sqlite-devel radiusclient-ng-devel portaudio-devel libresample-devel neon-devel libical-devel openldap-devel gmime-devel mysql-devel bluez-libs-devel jack-audio-connection-kit-devel gsm-devel libedit-devel libuuid-devel jansson-devel libsrtp-devel git subversion libxslt-devel kernel-devel audiofile-devel gtk2-devel libtiff-devel libtermcap-devel bison php php-mysql php-process php-pear php-mbstring php-xml php-gd tftp-server httpd sox tzdata mysql-connector-odbc mysql-server fail2ban
service iptables save
service iptables stop
chkconfig iptables off
sestatus
sed -i -e 's/SELINUX=permissive/SELINUX=disabled/g' /etc/selinux/config

```
