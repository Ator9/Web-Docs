Attach disk
========
https://learn.microsoft.com/en-us/azure/virtual-machines/linux/attach-disk-portal?tabs=ubuntu
```sh
apt install parted xfsprogs net-tools
```

Once connected to your VM, you need to find the disk. In this example, we're using lsblk to list the disks.
```sh
lsblk -o NAME,HCTL,SIZE,MOUNTPOINT | grep -i "sd"
```

Prepare a new empty disk
```sh
sudo parted /dev/sdb --script mklabel gpt mkpart xfspart xfs 0% 100%
sudo mkfs.xfs /dev/sdb1
sudo partprobe /dev/sdb1
```

Mount the disk
```sh
sudo mkdir /data1
sudo mount /dev/sdb1 /data1
```

Edit /etc/fstab

```sh
# get disk id
sudo blkid
```
```sh
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /data1   xfs   defaults,nofail   1   2
```

FQDN Domain
========
Edit /etc/hosts

```sh
10.10.10.10 webs1.domain.com webs1
```
Edit /etc/hostname

```sh
webs1
```
