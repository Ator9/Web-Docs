### SSH Key + mRemoteNG
https://technotes.khitrenovich.com/opening-ssh-aws-hosted-linux-servers-mremoteng/

## Rocky Linux 9 - ISPConfig Multiserver - Admin

# 1. Startup
```sh
my_db_pass=XXX

my_cp_host=XXX
my_cp_ip=XXX
my_http_host=XXX
my_http_ip=XXX
```

```sh
yum update -y
yum install -y telnet nmap quota ntp epel-release git yum-utils
yum install -y clamav clamav-update rkhunter
echo "$my_adm_ip     $my_adm_host" >> /etc/hosts
echo "$my_http_ip     $my_http_host" >> /etc/hosts
service network restart
ln -sf /usr/share/zoneinfo/America/Argentina/Buenos_Aires /etc/localtime
sed -i -e 's/PermitRootLogin yes/PermitRootLogin without-password/' /etc/ssh/sshd_config
service sshd restart
chkconfig ip6tables off

```
