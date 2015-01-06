CentOS 7 - Multiserver - HTTP Server
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
##<a href="https://github.com/Ator9/ISPConfig/blob/master/CentOS_adm.md#1-startup" target="_blank">1. Startup</a>
##<a href="https://github.com/Ator9/ISPConfig/blob/master/CentOS_adm.md#2-swap-quota-fail2ban--firewall" target="_blank">2. Swap, quota, fail2ban & firewall</a>
##<a href="https://github.com/Ator9/ISPConfig/blob/master/CentOS_adm.md#3-mariadb--grant-access-to-servers" target="_blank">3. MariaDB & GRANT access to servers</a>
##<a href="https://github.com/Ator9/ISPConfig/blob/master/CentOS_adm.md#4-apache--php" target="_blank">4. Apache & PHP</a>
##5. PureFTPd
```sh
yum install -y pure-ftpd
chkconfig --levels 235 pure-ftpd on
/etc/init.d/pure-ftpd start
mkdir -p /etc/ssl/private/
openssl req -x509 -nodes -days 7300 -newkey rsa:2048 -keyout /etc/ssl/private/pure-ftpd.pem -out /etc/ssl/private/pure-ftpd.pem
```
##6. Jailkit
```sh
yum install -y gcc
wget http://olivier.sessink.nl/jailkit/jailkit-2.17.tar.gz
tar -zxvf jailkit-2.17.tar.gz
cd jailkit-2.17 ; ./configure
make ; sudo make install
cd .. ; rm -rf jailkit-2.17*
```
##7. ISPConfig (Expert mode)
```sh
yum install -y php php-mysql php-odbcphp-mcrypt
wget http://www.ispconfig.org/downloads/ISPConfig-3-stable.tar.gz
tar -zxvf ISPConfig-3-stable.tar.gz
sudo php -q ispconfig3_install/install/install.php
```
