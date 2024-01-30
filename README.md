# epam_intro_cloud_devops

## 1. Linux introduction

### 1.1	Logical Volume Managent – LVM
Refers to the technology that allows to create units of virtual disks, mostly supported on linux

![image](https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/ad89ed63-3f13-4c0c-86db-776ca194eaa7)

To create a LVM you need yo perform the following tasks:
- Initialize partitions you will use for LVM
  
  `$ sudo pvcreate /dev/sbd1 #Partition must exist`
- Create volume group
  
  `$ sudo vgcreate vg_newlvm /dev/sbd1 #Create logical group from sbd1 pv`
- Create logical volume
  
  `$ sudo lvcreate –name centos7_newvol -l 100%FREE vg_newlvm #Creates a logical volume called centos7_newvol that uses all of the unallocated free space in the volume group vg_newlvm`
- Create a filesystem for the logical volume
  
  `$ sudo mkfs.ext4 /dev/vg_newlvm/centos7_newvol # Formats logical volume called centos6_newvol into ext4 filesystem`

### 1.2 SWAP 
When computer is out of RAM and needs to complete memory request allocation, it transfers inactive cold pages from the RAM to the SWAP space. SWAP space is recommended to be a partition but also can be used as SWAP files.

To create a SWAP partition as a LVM we need to complete the following:
- Create logical volume
  
  `$ sudo lvcreate volgroup00 -n logvol02 -L 2G #create lvm2 logical volume of size 2G from volume group volgroup00`
- Format the logical volume to be swap space
  
  `$ sudo mkswap /dev/volgruop00/logvol02`
- Add the following entry to the fstab file #The fstab file defines how filesystem should be mount during the boot process. It includes entries that defines the devices and partitions to be mounted as well as its mounting points and several mount options
  
  `$ /dev/volgruop00/logvol02 swap swap defaults 0 0`
-	Regenerate mount unis for the system to register the new conf
  
  `$ systemclt daemon-reload`
- Activate swap on the logical volume
  
  `$ swapon -v /dev/volgropu00/logvol02`
  
To create a SWAP file, we do the following:
-	Create and empty file of size 1 GB

 	`$ dd if=/dev/zero of=/swapfile bs=1024 count=1024000`
- Set up the swap file

  `$ mkswap /swapfile`
-	Change the access permissions of the swap file

 	`$ chmod 0600 /swapfile`
-	To enable swap file at the boot time, need to add it to the fstab file
  
  `$ /swapfile swap swap defaults 0 0`
-	Regenerate mount unis for the system to register the new conf
  
  `$ systemclt daemon-reload`
-	Activate swap file
  `$ swapon /swapfile`
  
### 1.3.Disk Quotas 
Allows the restriction in the amount of storage space used in a partition by a user or group. Also, can control the number of files created by specifying the maximum number of inodes in that user or user group filesystem
### 1.4	Boot Loaders
At computer start up, the OS system needs to be loaded from HDD or SDD storage devices. There is small program to do this, it is called the boot loader or bootstrap and it is usually store on the ROM of the device. For linux based systems we can find the GRUB boot loader.
### 1.4 Run Levels
It defines the state of the computer machine after booting. In practice, run level 0 meas the machine halts and run level 6 means the machine reboots.
