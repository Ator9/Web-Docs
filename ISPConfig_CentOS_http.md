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
##<a href="https://github.com/Ator9/Docs/blob/master/ISPConfig_CentOS_adm.md#1-startup" target="_blank">1. Startup</a>
##<a href="https://github.com/Ator9/Docs/blob/master/ISPConfig_CentOS_adm.md#2-swap-quota-fail2ban--firewall" target="_blank">2. Swap, quota, fail2ban & firewall</a>
##<a href="https://github.com/Ator9/Docs/blob/master/ISPConfig_CentOS_adm.md#3-mariadb--grant-access-to-servers" target="_blank">3. MariaDB & GRANT access to servers</a>
##<a href="https://github.com/Ator9/Docs/blob/master/ISPConfig_CentOS_adm.md#4-apache--php" target="_blank">4. Apache & PHP</a>
##5. PureFTPd & Jailkit
```sh
yum install -y pure-ftpd gcc
systemctl enable pure-ftpd.service; systemctl start pure-ftpd.service
wget http://olivier.sessink.nl/jailkit/jailkit-2.17.tar.gz
tar -zxvf jailkit-2.17.tar.gz
cd jailkit-2.17 ; ./configure
make ; sudo make install
cd .. ; rm -rf jailkit-2.17*

```
##<a href="https://github.com/Ator9/Docs/blob/master/ISPConfig_CentOS_adm.md#6-ispconfig-expert-mode" target="_blank">6. ISPConfig (Expert mode)</a>
##<a href="https://github.com/Ator9/Docs/blob/master/ISPConfig_CentOS_adm.md#7-configuration" target="_blank">7. Configuration</a>
##8. Server Status (Optional)
```sh
echo '<Location /status>' >> /etc/httpd/conf/httpd.conf
echo 'SetHandler server-status' >> /etc/httpd/conf/httpd.conf
echo 'Order Deny,Allow' >> /etc/httpd/conf/httpd.conf
echo 'Deny from all' >> /etc/httpd/conf/httpd.conf
echo "Allow from ${SSH_CLIENT%% *}" >> /etc/httpd/conf/httpd.conf
echo '</Location>' >> /etc/httpd/conf/httpd.conf
service httpd restart

```
##9. Secure PureFTPd (Optional)
```sh
mkdir -p /etc/ssl/private/
openssl req -x509 -nodes -days 7300 -newkey rsa:2048 -keyout /etc/ssl/private/pure-ftpd.pem -out /etc/ssl/private/pure-ftpd.pem

```
```sh
chmod 600 /etc/ssl/private/pure-ftpd.pem
echo "TLS    2" >> /etc/pure-ftpd/pure-ftpd.conf
systemctl restart pure-ftpd.service

```
