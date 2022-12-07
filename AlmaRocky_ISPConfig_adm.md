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
yum install -y nano git telnet nmap quota epel-release langpacks-en
yum install -y yum-utils clamav clamd clamav-update rkhunter

ln -sf /usr/share/zoneinfo/America/Argentina/Buenos_Aires /etc/localtime
sed -i -e 's/PermitRootLogin yes/PermitRootLogin without-password/' /etc/ssh/sshd_config
service sshd restart

echo "$my_cp_ip     $my_cp_host" >> /etc/hosts
echo "$my_http_ip     $my_http_host" >> /etc/hosts
```

# 2. Swap, quota, firewall
```sh
sudo fallocate -l 1G /var/swap.img ; chmod 600 /var/swap.img
mkswap /var/swap.img ; swapon /var/swap.img
echo "/var/swap.img    none    swap    sw    0    0" >> /etc/fstab

mount | grep ' / '
sed -i '0,/console=tty0/s//console=tty0 rootflags=uquota,gquota/' /etc/default/grub
grub2-mkconfig -o /boot/grub2/grub.cfg
reboot

systemctl stop firewalld.service ; systemctl disable firewalld.service
sed -i -e "s/=permissive/=disabled/g" /etc/selinux/config

```
