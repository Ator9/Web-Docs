CentOS Multiserver - Admin
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
