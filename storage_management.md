# Storage Management In Linux

**How the disk is divided into partitions and how the information about these partitions is stored?**\
MBR (Master Boot Record) and GPT (GUID Partition Table) are two different partitioning schemes used to organize and manage data on storage devices, particularly on hard disk drives (HDDs) and solid-state drives (SSDs).

**List, create, delete, and modify physical storage partitions**

*Lists information about block devices?*
```
sudo lsblk
sudo fdisk --list /dev/sda
```

*How to check whether your disk is using the Master Boot Record (MBR) or the GUID Partition Table (GPT) on Ubuntu*
```
sudo parted /dev/sda print
or
Install gdisk if it's not already installed
sudo apt-get install gdisk
sudo gdisk /dev/sda
```

```
lsblk
sudo cfdisk /dev/sdb
or 
sudo fdisk /dev/sdb
sudo lsblk -f
sudo lsblk
sudo pvcreate /dev/vda4
sudo vgdisplay
sudo vgextend ndc-vg /dev/vda4
sudo vgdisplay
sudo lsblk
sudo lvdisplay
sudo lvextend -L +50G /dev/ndc-vg/lv-var
sudo lvextend -l +100%FREE /dev/ndc-vg/lv-var
sudo resize2fs /dev/ndc-vg/lv-var
sudo df -h
sudo lsblk
```
**Create and configure file systems**

The mkfs command is used to create a new file system on a device (such as a partition or a disk).
`sudo mkfs.ext4 /dev/sdb1`

The tune2fs command is used to adjust various parameters and settings of an existing ext2, ext3, or ext4 file system
`sudo tune2fs -L MyVolume /dev/sdb1`

Mount /dev/sdb1 into /mnt directory
`sudo mount /dev/sdb1 /mnt`

Check block device ID
`sudo blkid /dev/sdb1`


**Configure systems to mount file systems at or during boot**

```
sudo vim /etc/fstab
#add this end of the line
/dev/sdb1   /mnt/   ext4   noauto   0   0

#If you want to mount via BLKID
UUID=1a11e12f-ca67-4235-89ca-dce3cb67c964   /mnt/  ext4   defaults   0   0

```

**Configure systems to mount file systems at or during boot**


sudo vi /etc/auto.master


Go to the end of the file and add this line:
/shares /etc/auto.shares


Next, create and edit this file:

sudo vi /etc/auto.shares
mynetworkshare -fstype=auto 127.0.0.1:/etc


sudo systemctl reload autofs



To verify your changes, you can try to run below given command:
ls /shares/mynetworkshare



You should see the contents of 127.0.0.1:/etc directory.

