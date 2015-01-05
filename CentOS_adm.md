CentOS 7 - Multiserver - Admin
```sh
my_ssh_user=XXX
```
#1. Startup
```sh
yum update -y
yum install -y telnet nmap quota ntp epel-release git
yum install -y clamav clamav-update rkhunter
yes | cp /usr/share/zoneinfo/America/Argentina/Buenos_Aires /etc/localtime
sed -i -e 's/#PermitRootLogin yes/PermitRootLogin without-password/' /etc/ssh/sshd_config
service sshd restart
adduser $my_ssh_user ; passwd $my_ssh_user
```

#2. Swap, quota, fail2ban & firewall
```sh
sudo fallocate -l 1G /var/swap.img ; chmod 600 /var/swap.img
mkswap /var/swap.img ; swapon /var/swap.img
echo "/var/swap.img    none    swap    sw    0    0" >> /etc/fstab
sed -i '0,/defaults/s//defaults,usrquota,grpquota/' /etc/fstab
mount -o remount /
quotacheck -avugm ; quotaon -avug
yum install -y fail2ban
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sed -i -e 's/bantime  = 600/bantime  = 3600/g' /etc/fail2ban/jail.local
systemctl enable fail2ban.service ; systemctl start fail2ban.service
systemctl stop firewalld.service ; systemctl disable firewalld.service
```

#3. Install MariaDB
secure_install: yes to all questions
```sh
yum install -y mariadb-server
service mariadb start ; systemctl enable mariadb.service
/usr/bin/mysql_secure_installation
```

#4. Install Apache & PHP
Document root: /var/www/html
Configuration files: /etc/httpd/conf.d/
```sh
yum install -y httpd mod_ssl
service httpd start ; systemctl enable httpd.service
yum install -y php php-devel php-gd php-imap php-ldap php-mssql php-mysql php-odbc php-pear php-xml php-xmlrpc php-pecl-apc php-mbstring php-mcrypt php-snmp php-soap php-tidy curl curl-devel perl-libwww-perl ImageMagick libxml2 libxml2-devel php-cli httpd-devel unzip bzip2 perl-DBD-mysql php-fpm mod_fcgid
systemctl start php-fpm.service ; systemctl enable php-fpm.service
service httpd restart
```
