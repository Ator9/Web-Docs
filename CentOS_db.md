CentOS 7 - Multiserver - Database
```sh
my_ssh_user=XXX
my_db_pass=XXX

my_adm_host=XXX
my_adm_ip=XXX
my_db_host=XXX
my_db_ip=XXX
my_http_host=XXX
my_http_ip=XXX
```

#1. Startup
#2. Swap, quota, fail2ban & firewall
#3. MariaDB & GRANT access to servers

#4. ISPConfig (Expert mode)
```sh
yum install -y perl-DateTime-Format-HTTP perl-DateTime-Format-Builder perl-Time*
service httpd restart
wget http://www.ispconfig.org/downloads/ISPConfig-3-stable.tar.gz
tar -zxvf ISPConfig-3-stable.tar.gz
sudo php -q ispconfig3_install/install/install.php
```
