Attach disk
========
https://learn.microsoft.com/en-us/azure/virtual-machines/linux/attach-disk-portal?tabs=ubuntu

Once connected to your VM, you need to find the disk. In this example, we're using lsblk to list the disks.
```sh
lsblk -o NAME,HCTL,SIZE,MOUNTPOINT | grep -i "sd"
```
Mount the disk
```sh
sudo mkdir /data1
sudo mount /dev/sdb /data1
```
