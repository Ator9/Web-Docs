CentOS 7 - GitLab
```sh
my_ssh_user=XXX
```
#1. Startup
```sh
history -c
yum update -y
sudo fallocate -l 1G /var/swap.img ; chmod 600 /var/swap.img
mkswap /var/swap.img ; swapon /var/swap.img
echo "/var/swap.img    none    swap    sw    0    0" >> /etc/fstab
yes | cp /usr/share/zoneinfo/America/Argentina/Buenos_Aires /etc/localtime
sed -i -e 's/#PermitRootLogin yes/PermitRootLogin without-password/' /etc/ssh/sshd_config
service sshd restart
adduser $my_ssh_user ; passwd $my_ssh_user
```

#2. fail2ban 
```sh
yum install -y fail2ban
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sed -i -e 's/bantime  = 600/bantime  = 3600/g' /etc/fail2ban/jail.local
systemctl enable fail2ban.service ; systemctl start fail2ban.service

```

#3. GitLab <a href="https://about.gitlab.com/downloads/" target="_blank">URL</a>
```sh
firewall-cmd --permanent --add-service=http
systemctl reload firewalld
curl -O https://downloads-packages.s3.amazonaws.com/centos-7.0.1406/gitlab-7.7.2_omnibus.5.4.2.ci-1.el7.x86_64.rpm
rpm -i gitlab-7.7.2_omnibus.5.4.2.ci-1.el7.x86_64.rpm
gitlab-ctl reconfigure

```
