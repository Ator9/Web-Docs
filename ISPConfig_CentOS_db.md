CentOS 7 - Multiserver - Database
## <a href="https://github.com/Ator9/Docs/blob/master/ISPConfig_CentOS_adm.md#1-startup" target="_blank">1. Startup</a>
## <a href="https://github.com/Ator9/Docs/blob/master/ISPConfig_CentOS_adm.md#2-swap-quota-fail2ban--firewall" target="_blank">2. Swap, quota, fail2ban & firewall</a>
## <a href="https://github.com/Ator9/Docs/blob/master/ISPConfig_CentOS_adm.md#3-mariadb--grant-access-to-servers" target="_blank">3. MariaDB & GRANT access to servers</a>
## 4. PHP & ISPConfig
Expert mode (firewall y, other n)
```sh
yum install -y php php-mysql php-odbcphp-mcrypt php-mbstring
wget http://www.ispconfig.org/downloads/ISPConfig-3-stable.tar.gz
tar -zxvf ISPConfig-3-stable.tar.gz
sudo php -q ispconfig3_install/install/install.php

```
## 5. MariaDB Configuration (Optional)
Private Network Access
```sh
sed -i -e "s/\[mysqld\]/\[mysqld\]\nbind-address = $my_db_ip/g" /etc/my.cnf
service mariadb restart

```
Max Connections
```sh
sed -i -e 's/\[mysqld\]/\[mysqld\]\nmax_connections = 500/g' /etc/my.cnf
service mariadb restart
```
Backup Cron
```sh
55 * * * *   root    mysqldump -uXXX -pXXX dbname | gzip > /backups/dbname.sql.gz
```
