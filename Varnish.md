# Varnish
nano /etc/yum.repos.d/varnishcache_varnish60lts.repo
```sh
[varnishcache_varnish60lts]
name=varnishcache_varnish60lts
baseurl=https://packagecloud.io/varnishcache/varnish60lts/el/7/$basearch
repo_gpgcheck=1
gpgcheck=0
enabled=1
gpgkey=https://packagecloud.io/varnishcache/varnish60lts/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300

[varnishcache_varnish60lts-source]
name=varnishcache_varnish60lts-source
baseurl=https://packagecloud.io/varnishcache/varnish60lts/el/7/SRPMS
repo_gpgcheck=1
gpgcheck=0
enabled=1
gpgkey=https://packagecloud.io/varnishcache/varnish60lts/gpgkey
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
metadata_expire=300
```
```sh
sudo yum -q makecache -y --disablerepo='*' --enablerepo='varnishcache_varnish60lts'
yum install varnish
varnishd -V
```
nano /etc/varnish/varnish.params
```sh
VARNISH_LISTEN_PORT=80
```
nano /etc/varnish/default.vcl
```sh
backend default {
    .host = "127.0.0.1";
    .port = "81";
}
```
nano /lib/systemd/system/varnish.service
```sh
ExecStart=/usr/sbin/varnishd -a :80 -f /etc/varnish/default.vcl -s malloc,256m
```
```sh
sudo systemctl daemon-reload
sudo service varnish restart
```
