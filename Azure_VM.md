Attach disk
========
https://learn.microsoft.com/en-us/azure/virtual-machines/linux/attach-disk-portal?tabs=ubuntu
```sh
apt install parted xfsprogs
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
