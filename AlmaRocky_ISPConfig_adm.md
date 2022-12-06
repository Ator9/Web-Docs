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

echo "$my_adm_ip     $my_adm_host" >> /etc/hosts
echo "$my_http_ip     $my_http_host" >> /etc/hosts
service network restart

```

# 2. Swap, quota, fail2ban & firewall
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
sed -i -e "s/=permissive/=disabled/g" /etc/selinux/config

```
