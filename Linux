# epam_intro_cloud_devops

## 1. Linux introduction

### 1.1	Logical Volume Managent – LVM
Refers to the technology that allows to create units of virtual disks, mostly supported on linux

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/ad89ed63-3f13-4c0c-86db-776ca194eaa7"
         alt="Figure 1" width="600" height="300"/>
  <br/>
  <em>Figure 1. LVM Architecture</em>
</p>

To create a LVM you need yo perform the following tasks:
- Initialize partitions you will use for LVM
  
  `$ sudo pvcreate /dev/sbd1 #Partition must exist`
- Create volume group
  
  `$ sudo vgcreate vg_newlvm /dev/sbd1 #Create logical group from sbd1 pv`
- Create logical volume
  
  `$ sudo lvcreate –n centos7_newvol -l 100%FREE vg_newlvm #Creates a logical volume called centos7_newvol that uses all of the unallocated free space in the volume group vg_newlvm`
- Create a filesystem for the logical volume
  
  `$ sudo mkfs.ext4 /dev/vg_newlvm/centos7_newvol # Formats logical volume called centos6_newvol into ext4 filesystem`

- Mount LVM to somewhere for instance in /mnt/data (the directory must exist)
  `$ mount /dev/vg_newlvm/cento7_newvol /mnt/data`

- To make the mounting point permanently of the lv we need to add the entry to the fstab file. This file manages how devices, partitions and other devices are mounted into the system
  `$ /dev/vg_newlvm/cento7_newvol /mnt/data ext4 defaults 0 2`

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
It defines the state of the computer machine after booting. In practice, run level 0 means the machine halts and run level 6 means the machine reboots. 

Modern distributions use systemd daemon as process and service manager. Systemctl is a command-line utility that is used to control and manage systemd system and service manager. Runlevels can be set by using systemctl command
- To list all run levels from systemd we an do:
  
  `$ systemctl list-units --type=target`
- To get the default run level we can do:
  
  `$ systemctl get-default`
- To set the default run level we can do:
  
  `$ systemctl set-default runlevel3.target`

- To change to a specific run level we can do:
  
  `$ systemctl isolate runlevel0.target # This command shuts down the machine`

## 2. Linux Shell Basics

### 2.1 Shell
A shell is a command line interpreter that allows user to input commands that execute some tasks. This commands are translated into calls to the linux kernel so the instructions can be completed by the underlying hardware. Commands also can be read from a file and in this way is called scripting. There are various types of shells like sh (bourne shell), csh (C shell), tcsh (Tenex C shell - Turbo C shell), ksh (Korn shell) and others yet the standard for linux distros is bash (Bourne again shell) which is a superset of sh adding more flexibility and advanced features to users.

The shell can be customized to display information in the shell window as preferred by the user, also can be used to execute script files when login in and login out. Common files to set behaviour of the bash shell are:
- /etc/profile
- ~/.bash_profile
- ~/.bash_login or ~/.profile
- ~/.bash_logout

### 2.2 Shell programming
Shell commands can be used to create scripts to automate certain tasks. Shell scripting is common used for repetitive operations over the linux distro like for example checking the disk space and if over 70% write a log that goes to a logging server. Despite being a powerful tool, we shouln't consider to use shell scripting to build complex applications since it doesn't support OOP or other complex data structures such as multidimensional arrays, linked lists, stacks, trees and so on. 

### 2.3 Basic Shell commands
- man, used to get information about commands, utilities and other programs in the system
  
  `$ man -k command_name #Searches for commands that contains the command_name in their name`\
  `$ man -f command_name #Searches for commands that their name is command_name`
- info, used to display information about commands

  `$ info command #Searches for documentation of command`
- mkdir, used to create directories
  
    `$ mkdir -p par_dir/child_dir{a..c}/child_dir # when used with the -p option it creates the parent directories of child directories in case they don't extist`

- mv, used to move files/directories to another location

  `$ mv -b -S ".old" filename dir/ # The option -b means backup, if filename already exists in the destination, the existing file will be rename with the suffix indicated by the option -S`
- tail, to view the last 10 lines by default of a file
 
     `$ tail -f # when used with the -f option it display interactiely the contents of the file, useful when checking dynamic file such as logs`
    
### 2.4 Text editors

Text editor are used to create and edit plain text files. All linux distros support vi, vim and nano and by default comes with vi installed.

### 2.5 Filesystem files search

When need to search for a file or directory within a the filesystem structute we can use three different commands: find, locate and grep

- find: support various options to narrow down the search to find items based on its type like a regular file or a directory. It mainly work with the path that is given to complete the search, if the path is not specified by default searches all filesystem under /

  `$ find /tmp -name core -type f -exec rm {} \; # The commands searchs within the /tmp folder a file with name core and then removes it`

- locate: uses local database mlocate to quickly find locations of files or directores within the filesystem, mlocated is an indexed database located at /var/lib/mlocate with file names and therefore it is much faster to lookup for them since it does not the to scan the entire filesystem but rather find the corresponding entry in the mlocate.db file. The database is updated regularly but can be done by using the command updatedb.

  `$ locate -br ^shadow$ # the command searches for filenames which their base contains only the word shadow`

- grep: can be used to search for specific matches of contents in plain files using regular expressions. For instance maybe you want to find a file that you remember it had a definiton of a function called myfunc(). Since grep is to find patterns in files you need to specified the file to read from, if not specified it will read from STDIN

`$ grep -r myfun . # With the option -r searches recursively the pattern myfun inside the plain files living under the specified path. The dot means to search in the current directory`

### 2.6 Xargs

Command that reads arguments from SDTIN and executes the specified command for each of given arguments. If no command is given form xargs then the default command it uses is /bin/echo

We see a couple a couple of examples here:

- Command to find all *.png files and compress them

  `$ find . -name *.png -type f | xargs tar -cvzf images.tgz`
- Command to list in a single line all users in your linux distro:
  
  `$ cut -d ":" -f 1 /etc/passwd | xargs`

- Command to create files with arguments as filenames

  `$ cat name | xargs touch`

- Command to see the number of lines, words and bytes files have

  `$ ls -p  | grep -v / | xargs wc`

- Command to recursively delete folder that contain certain name

  `find /Downloads -name downterm -type d print0 | xargs -0 rm -vrf "{}" # print0 print output with null character as spacing instead of withespace, the -0 option of xargs command indicates the arguments are expected to be separated by the null character, the {} means the arguments from SDTIN`

- Command to copy executable files from one location to another

  `$ find -name *.sh -type f print0 | xargs -0 -i cp {} target_folder`

 ### 2.7 Working with archives

In linux there are various utilities for achiving and compressing files. Archiving means putting togheter various files into a single one, think of it as putting various objects into a single box. By the other hand compressing means reducing the size in disk of an object. There various compressing algorithms that can be used, one important thing to consider is whether the algorithm is lossy or lossless. Lossy algorithms can be use for instance for compressing data types that permit certain loss of their information such an image, audio or video whereas lossless compressing must be used for data that cannot admit any loss and their integrity must remain unchanged such as binary files, text files and scripts.

Tools we can use for both purposes of archiving and compresing are tar, zip and gzip. GZIP can achieve higher compression ratios that ZIP because it uses DEFLATE compression algotithm, however gzip can be applied to singe files or directories only. Whereas ZIP can combine various files and directories into one single compressed and archived file. Commonly command tar with compression option -z that means bzip is used.

- Archiving various files and compressing with bzip the resulting object

  `$ tar -cvzf images.tgz *.png #The -c option means create archive, -v means verbose output, -z means compress by using lossless bzip algorithm, -f specifies the name of the resulting file and finally the oputput is`

- List archived files in archive

  `$ tar -tf image.tgz`

- Decompressing and unarchiving files from compressed archived file object

    `$ tar -xvzg images.tgz # -x option means extract`

## 3. Package Management
### 3.1 Sofware Management
In Linux there is a wide range of utilities, services and other tools available in the form of software package. A software package is binary archive that is available for download and installation on the local linux system. Such archived software files can be managed by a software management system comprised of a set of tools that can automate the installation, updating, upgrading, purging and removal of such software packages. Each distribution family can have their own software package management system, for example in Readhat derived distros we can find RPM and YUM managers whereas in Debian derived distros we can use PKG and APT. Both RPM and PKG are considered to be low level package management tools to manage individual software packages, also they they cannot resolve dependencies (if needed) by a package, this means that you must install manually all dependencies before hand required by a particular software package. YUM and APT on the other part, manage dependency resolution automatically and make all dependencies installations for you, underneath they also use RPM and PKG as their backend for individual package management.

Repository management:

In centos, all repositories are located in /etc/yum.repos.d/ as configuration files

- `$ sudo yum repolist` Lists enabled repositories in your system
- `$ sudo yum repolist all` Lists enabled and disabled repositories in your system
- `$ sudo yum-config-manager --enable/--disable <repo_name>` Enables/Disables repository
- `$ sudo yum-config-manager --add-repo <repo_url>` adds repository
- `$ sudo yum --disablerepo="*" --enablerepo="ksplice-uptrack"` List available packages under repo ksplice-uptrack
- `$ sudo yum history` Displays the history of transactions made with yum
- `$ sudo yum history info <transaction_id>` Displays information about a particular transaction made with yum
- `$ sudo yum history undo <transaction_id>` Undoes transaction made with yum
- `$ sudo yum history redo <transaction_id>` Redoes transaction made with yum

You can also create a customized repo under /etc/yum.repos.d/ by doing the following:

- `$sudo vi /etc/yum.repos.d/myrepo.repo` Creates a repository configuration file named myrepo.repo
- Edit repo configuration file
  ```
  [repository_name]
  name=Repository Name
  baseurl=http://example.com/repo
  enabled=1
  gpgcheck=1
  gpgkey=http://example.com/RPM-GPG-KEY-repo```
- `$ sudo yum makecache` Update the yum cache to make the new repository available
  
Package management:

- `$ yum list installed <package>` List installed package version, if package is not given lists all packages installed on system
- `$ yum search <package>`Search package in enabled system repositories
- `$ yum provides <command|file>` Search what package provides the command or file
- `$ sudo yum autoremove <package>` Removes a package and all its unused dependencies
- `$ sudo yum check-updates` Check if packages have new version available
- `$ sudo yum update <package>` Updates specified package, if not given then updates all packages with new version available
- sudo yum remove --purge <package_name> Removes a package and its installed files
- `$ rpm -qa | egrep <regex>` Lists all installed packages and filter by regex
- `$ rpm -qf $(which command)` Check what package the specified command belongs to
- `$ rpm -ql <package>)` Check what files were installed by package
- `$ rpm -q --requires <package>` Lists dependencies needed by package
  

### 3.2 Alternatives

The alternatives command in Linux is used to manage symbolic links for different versions of a program or service. It allows you to switch between different implementations of a particular command or service without manually updating the symbolic links. Normally alternatives command can be used for:

- Switching between different versions of a program such as for example switching between python 2 and python3 version
- Setting default programs such as the default text or mail client
- Setting system-wide configuration parameters such as default python version

### 3.3 Systemd/init.d

In legacy system-v based linux systems the scripts for managing system services can be found in the /etc/init.d folder. To manage a particular system service we can  run `$ /etc/init.d/service_name command` where service_name is the service to be managed and command can be one of the following:
- Start
- Stop
- Restart
- Reload
- Force-reload
  
In newer linux systems the process and service manager is called systemd. This modern system initialization and process and service manager tool can offer faster boot times compared to system-v by starting system services in parallel. Systemd uses unit files located in **/usr/lib/systemd/system** (units created by installation packages), **/run/systemd/system** (units created at runtime) and **/etc/systemd/system** (system-wide units and custom units
created by user) that describes the metadata, resources, commands,  and dependencies for managing the execution of the system services. Unit files follows a declarative structure comprised of three main parts

1. [unit]: Contains generic (metadata) information about the service
2. [service]: Which encodes information about the service itself. It contains all those settings that apply only to services, the start, stop, reload script
3. [Install]: It encodes information about how the suggested installation should look like, for instance, under which circumstances and by which triggers the service shall be started.

For intereacting with a service we can run:

- Status, start, stop, restart, reload service

  `$ systemctl status|start|stop|restart|reload sshd.service`

Some services and applications are needed to start automatically at boot time, to achieve this:

- `$ sudo systemctl enable|disable <daemon|service unit file>` Enables|Disables to start automatically at boot the daemon or the service unit file
- `$ sudo systemctl is-enable <daemon| service unit file>` Checkes whether the specified daemon or service is configured to automatically start at boot time
- `$ sudo systemctl list-unit-files --state=enabled` Lists all unit files that are configured to automatically start at boot time

Systemd also uses its own logging daemon called journal that collects logs from various sources and stores them in form of binary files, this way allows effient search and filtering. To interact with the binary log files from journal we can used journalctl. For example to look for the status of a particular service we can run:

  `$ journalctl  -p err -b -xu sshd # -p means show only err (5) messages and above, -b means since last boot, -x means explanatory log ouput, -u means from which unit we want to see the log message`

### 3.3 Crond

Crond is a daemon that comes with all linux distros and it's useful for scheduled command execution. Crond searches for system-wide crontab files located in **/etc/crontab**, **/etc/anacrontab*** and **/etc/cron.d** and also searches for user defined crontabs files usually located at **/var/spool/cron** . To add/edit an user defined scheduled job you can use `$ crontab -e` this will open crontab file of the user in **var/spool/cron** folder. It is recommended to use crontab command instead of editing the files directly. 

The structure of an scheduled job in a crontab file is as shown in figure 2, the basic entry syntax is **"\* * * * * /path_to/{command|script
}"**. Dependending on the needs the scheduled jobs can be set to be executed on certaing basis either in terms of minutes, hours, days, months, weekdays or a whole combination of each item. The output of a crontab scheduled job is by default sent to the local mailbox in **/var/spool/mail/username** of the user that created the entry in the crontab file. If the output needs to be displayed or stored somewhere else then STDOUT redirection must be specied in the crontab entry.

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/656bd95a-f333-4d7e-aa1a-7a890623ddd7"
         alt="Figure 2" width="600" height="200"/>
  <br/>
  <em>Figure 2. Crontab entry syntax</em>
</p>

Some examples of scheduled jobs using crontab systax are:

- Execute echo command every 3 minutes and redirect its STDOUT and SDTERR to file

  `*/3 * * * * /bin/echo "Executed commmand" 2>&1 outputfile.log`
  
- Execute echo command at 05:00 PM from monday to friday during the month of March

  `0 17 3 * 1-5 /bin/echo "Executed commmand"`

- Execute echo command twice a day at 5 AM and 5 PM on the first sunday of every month

   `0 5,17 * * sun [ $(date +%d) -le 7 ] && /bin/echo "Executed command`

- Execute multiple task each 20 seconds

   `* * * * * /bin/sleep 20 && /path_to/script1.sh;/path_to/script2.sh;/path_to/script3.sh
## 4. Users, groups, permissions
### 4.1 Users and Groups

Users information in linux is stored in **/etc/password** file. This file contains login and non-login users. Non login users mean they are not intended to login into the system, these users are intended to control running background services, daemons, or system processess. For instace the postgresql user is used to manage execution of postgresql application. The sctructure of a entry in the **/etc/password** file is as shown in the figure 3. Each entry denotes a user.

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/c62ed4d2-85d6-4547-9b85-13f8452107e4"
         alt="Figure 2" width="600" height="200"/>
  <br/>
  <em>Figure 3. User information structure in /etc/password</em>
</p>

Groups infomation in linux is stored in **/etc/group** file. Each entry in this file represents a group. The structure of a entry in the **/etc/group** file is as shown in the figure 4.

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/d06155e0-fe10-431b-991d-9f786151ccca"
         alt="Figure 2" width="600" height="200"/>
  <br/>
  <em>Figure 4. Group information structure in /etc/group</em>
</p>

Commands for users:
- `$ su -l username` Switch between users. If not user is given by default tries to swicth to the root user.
- `$ sudo -u username command` Run commands with the security privilege of another user. If username if not given the instruction will be run as root.
- `$ useradd -d /home/username -s /bin/bash username` Adds username with home directory and shell specified by the options -d and -s respectively.
- `$ userdel -r username` Deletes username and its home directory.
- `$ usermod -d /home/new username` Modifies username's home directory
- `$ finger username` Displays information about an user

Commands for groups:
-`$ groupadd groupname` Adds group
-`$ groupdel groupname` Deletes group
-`$ groupmod -n newgroup groupname` Modfies group's name from groupname to newgroup

### 4.2 Permission Model

Each object under linux file system is accessible by its permission set. This permision set adds a security layer for access control, this limits what users and groups are allowed to access, write or execute the contents of the object. The figure 5 shows the permission model for linux objects

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/146394ed-d265-4b3b-8224-b5d7c78d1215"
         alt="Figure 2" width="600" height="200"/>
  <br/>
  <em>Figure 5. Permission Model</em>
</p>

Commands to change permission on objects

- `$ chmod go-rw file.txt` Remove read and write permissions from group and others
- `$ chown new_user:new_gruop file.txt` Change the user and group owner of file.txt to new_user and new_group respectively.
- `$ chgrp new_group file.txt` Changes only the owner group of the file

When working in a shared folder by multiple users who has access rwx permissions in the folder's **group/others permission set**, the contents created by an individual user may be edited or deleted by another user. To prevent this situation there is something called the sticky bit, this is an extra permission value that can be added to a shared folder in the **others** permission set, this means that files created by individual users cannot be changed or deleted by other users. The sticky bit can be recognized in a shared folder when the **others** permission set contains a t or T. If T is displayed it means the others permission set is missing the execute permission (x), meaning that users that don't belong to the shared_folder group cannot enter the directory.

- `$ chmod +t shared_folder` or `$ chmod o+t shared_folder` Adds the stickly bit for the shared folder in symbolic notation
- `$ chmod -t shared_folder` or `$ chmod o-t shared_folder` Removes the stickly bit for the shared folder in symbolic notation
- `$ chmod 1755 shared_folder` Adds the stickly bit for the shared folder in octal notation
- `$ chmod 0755 shared_folder` Removes the stickly bit for the shared folder in octal notation

There are other two extra permissions called **Set User ID (SUID)** and **Set Group ID (SGID)**, these permissions allows effectively for a command or script to be run by another user on behalf of the user who owns the command/script. For this permission to be effective the x permission also needs to be set on the file.

- `$ chmod u+xs file.sh` or `$ chmod 4755 file.sh` Adds the SUID bit for file.sh meaning any user can run this command on behalf of user owner
- `$ chmod g+xs file.sh` or `$ chmod 2755 file.sh` Adds the GUID bit for file.sh meaning any user can run this command on behalf of group owner 

Finally, for files and directories there is a extended permission set that is called special attributes. These attributes can be set by chattr and lsiter by lsattr

- `$ lsattr filename/directory` Shows the special attributes set on the specified file or directory.
- `$ chattr +i filename` Sets the special attribute flag i which means inmutable, file cannot be change or deleted, not even by root
- `$ chattr +a filename` Sets the special attribute flag a which means content can only be added to file, previous content cannot be deleted, not even by root
- `$ chattr +s filename` Sets the special attribute flag s which means when secure deletion, the storage block occupied by the file on disk will be filled with zeros

### 4.3 SELinux

Security Enhanced Linux is an advanced access control mechanism built into most of morden linux distros. SELinux implements a fined-grained access control referred to as Mandatory Access Control (MAC), it uses policies under **/etc/selinux** that describe configurations and rules to control how users and processes interact with system resources such as files, directories, networks, ports and devices by restricting their permissions and actions over each resource.

SELinux has 3 working modes:

1. Enforcing: SELinux enforces its policy blocking any unauthorized access attempts by users and processes. The access denials are blocked and reported to relevant log files.
2. Permissive: Doest not enforces its policy and therefore no access is denied but any violation attempts are reported to relevant log files.
3. Disable: Does not enforces its policy and does not report any violation attempt.

To check the current status of SELinux we can use `$ getenforce` or `$ sestatus` commands

## 5. Networking, Remote Access

### 5.1 Network configuration

Interace networking configuration can be performed to be temporary or permament. Temporary configuration means changes are loaded in RAM during the current session, once the system shutdowns or reboots the configuration is lost. On the other hand permanent configuration means changes are stored in system files from which information is read in each boot. 

Temporary networking configuration can be achieve by using the `$ ip ` command. Some examples are:

- `$ ip address` Displays current interface address configuration
- `$ ip link show` Displays available interface cards and their statuses
- `$ ip link set enp0s8 down` Turning down an interface card
- `$ ip address add 192.16.15.1/24 dev enp0s8` Adds an static ip address to interface enp0s8
- `$ ip route add 172.16.20.0/24 via 192.16.15.2 dev enp0s8 permanent` Adds a permanent static route for net 172.16.20.0/4 whose next hop is 192.16.15.2 and the outbound interface is enp0s8

Permanent interface networking configuration must be done in system files. For centos these configuration files are interface specific scripts and can be found in **/etc/sysconfig/network-scritps/ifcfg-interface_name**. Each configuration script contains a set of directives in the form of key value pairs that describe the networking configuration of that particular interface. The following are some directives that can be configured:

- `BOOTPROTO=none|dhcp` Specifies whether the interface must request a dhcp address or uses local configuration contained in the script
- `IPADDR=192.168.15.1` Sets static IPV4 network address
- `NETMASK=255.255.255.0` Sets static IPV4 network mask
- `BROADCAST=192.168.15.255` Specifies the broadcast address to which the interface belongs
- `GATEWAY=192.168.15.100` Specifies the default gateway or router for the net to which the interface belongs
- `DNS=8.8.8.8` Specifies the default DNS used by the interface for domain resolution

### 5.2 Networking tools

Linux has various utilities for network related operations such as connectivity testing, packets collection and domain name resolution. The following are some of them:

- `$ ping -c 5 google.com` checks whether host is reachable
- `$ tracepath google.com` display the path the packages take to reach the destination (may not be the same every time)
- `$ host google.com` and `host 8.8.8.8` Translates domain names to ip addresses and viceversa, simple tool
- `$ dig google.com` and `dig 8.8.8.8` Translates domain names to ip addresses and viceversa, advanced tool for troubleshooting DNS and performing advanced queries
- `$ nslookup google.com` and `nslookup 8.8.8.8` Translates domain names to ip addresses and viceversa, being replaced by dig
- `$ tcpdump -i enps08` shows traffic being handled by interface enp0s8
- `$ ss -tlnp` shows system listening connections on tcp ports

### 5.3 Firewal.d and Iptables

Centos has default built-in stateful Firewald daemon installed. Firewald is a service that provides a dynamically managed firewall that allows for implementing network security. It allows the isolation of system networks resources on different zones. Each zone has a security level of trust which is more or less permissive with the network traffic going through according to the the zone definition. The zones to which a network resource can belong to in a system running firewalld are:

- Drop: Drops all incomming traffic and does not reply with any icmp message. Outgoing connections are possible
- Block: Similar to drop but replies with impv4-host-prohibited or icmpv6-adm-prohibited message after dropping incomming connections
- Public: Untrusted network, you can allow through certain traffic on a case-by-case basis.
- External: If you are using the firewall as a gateway but want to expose some internal resources, you set destination NAT for certain private resources to be reachable by public networks. Private resources remain private and firewall perform destination nat.
- Internal: If you are using the firewall as a gateway and want to access the internet, you set source NAT for address translation from private to public
- Dmz: Computer services that can be accesed by external networks, servers/computers located in this zone will not have access to internal network, only specified connections can be allowed.
- Work/home/trusted: Trusted networks

Firewalld keeps close relation with iptables command, as most of the configurations made by high level tools like firewall-cmd or firewall-config translates the rules into iptables commands. Figure 6 illustrates the architecture of firewalld, it can be seen that this service uses iptables command for implementing the security rules into the linux kernel.

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/6769e0d9-fc30-434d-870c-cd9d7439c8ab"
         alt="Figure 6" width="600" height="200"/>
  <br/>
  <em>Figure 6. Firewald and iptables service</em>
</p>

One key difference between firewalld and iptables service (not command) is that with the later adding or modifying any rule means flushings all current rules and recreating new ones from the files **/etc/sysconfig/itpables** and **/etc/sysconfig/ip6tables**, this causes active connections to be dropped. Firewalld by the contrary does not recreate all rules, instead only the difference of old configuration and new configuration is applied and therefore no connections are lost

Some commands to interact with firewalld:

- `$ firewall-cmd --get-services | xargs -n 1` Get all services listed line by line
- `$ firewall-cmd --info-service=ftp` Get information of an specific service
- `$ firewall-cmd --panic-on|off` Dropping all packets

  ### 5.4 Remote Access

Remote access is typically intented for performing administrative tasks on a remote machine. There are various protocols and GUI programs that can help in the establishment of remote connections. Some of them are:

1. Protocols
  - Telnet (insecure, not to be used over public networks)
  - SSH (Recommended, it uses encryption)
2. GUI programs
  - winSCP
  - Putty
  - X11 (Protocol to forward GUI of a remote machine)
  - VNC/Anydesk

Most of the time you will be using SSH utility to connect ot a remote host by using command line. The usage of SSH is as follows:

- `$ ssh-keygen -t rsa -c 4096` Generates ssh public-private key pair using RSA algorithm, the lenght of the key is set to be 4096 bits
- `$ ssh-copy-id remote@domain.com` Copies the pub key of current user to remote server, this allows for passwordless auth, when copied you can edit /etc/ssh/sshd_config file and set passwordauthentication to no
- `$ ssh -i ~/.ssh/id_rsa remote@domain.com` Logins to remote machine using private key file indicated by -i option
- `$ ssh remote@domain.com whoami&&pwd` Logins and executes the specified commands in the remote machine, then brings the output and terminates the session
- `$ ssh -t remote@domain.com sudo ls /root` if command in the remote machine needs users input a pseudo terminal must be added to provide the required action. For this purpose use -t option
- `$ ssh remote@domain.com < script.sh` Execution of local script in remote machine

When accesing various machines recurrently, you can edit the **~/.ssh/config** file for establishing alias for ssh connections, for example you can use `$ ssh remote_prod_server1` instead of using the full username and domain address. The following is an example:

```
host remote_prod_server1
    user remote_user
    hostname domain.com
    identityfile ~.ssh/id_rsa
    port 2222
```

Lastly, a very useful concept is about bastion host. This scenario is when you use an intermediary computer/server to login to another computer/server. Think of it as a "cascade login". For example, user in host A logins to host B and then from host B logins to host C. The command to achieve such behaviour with SSH is a proxy command. 

- `$ ssh -i key_for_hostB -o "ProxyCommand ssh -W %h:%p -i key_for_hostC host_c@hostc_domain.com" hostb@hostb_domain.com` The -o option in the ssh command means that a proxycommand option will be used.

## 6. Monitoring, Process Control, Logs

### 6.1 Process Monitoring

Processes are running programs or services that uses system resources such as processor, disk, network and RAM . To manage processes there are various utilities on linux that are: Top, ps, kill and killall.

1. Top: Dynamically shows proccesses information running on the system, the displayed information includes the number of tasks running, sleeping, stop and zombie, the PID (Process ID) of each task, the user that initiated the task, the duration in time since the process was started on the system, the command that initiated the process and so on. The top command also allows for interaction to organize process by %CPU and %MEM (RAM) consumption and displays the average load of the system in last 1, 5 and 15 minutes. CPU and RAM usage.

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/c00fe8ec-67cc-4196-bbac-61010f1b35f6"
         alt="Figure 7" width="600" height="200"/>
  <br/>
  <em>Figure 7. Example of Top command output</em>
</p>

2. Ps: Shows an snapshot of processes/services running in the system. It is a static version of TOP

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/0bfb526a-4e87-4227-b50a-805ce7cc3c8f)"
         alt="Figure 8" width="600" height="200"/>
  <br/>
  <em>Figure 8. Example of ps aux command output</em>
</p>

3. Kill/pkill: Allows to terminate a process or process group by sending a termination signal to it. Processes can be killed by using its process id or command name that initiated the process. If no signal is specified then the default signal sent is SIGTERM which allows the process to exit gracefully and to perform some clean up operations or shutdown process if needed. Another common signal for killing processes is SIGKILL, this signal terminates the process inmediatly, the general recommendation is to use SIGTERM and use SIGKILL as a last resort.

Some usefull commands are:

- `$ kill -l` Lists all available termination signals. Processes may react differently to a given termination signal, for instace the signal SIGHUP is normally used for daemons to reload configuration files.
- `$ kill 156` kills with SIGTERM signal the process id 156
- `$ pkill -9 ping` kills with SIGKILL signal all the processes of name ping
   
### 6.2 Monitoring CPU

For interacting with the Central Processing Unit we can use several commands to show information, CPU load and CPU statistics.

- `$ lscpu` This commands shows architecure of the CPU such as the architecture (generally _X86 in servers), number of cores, sockets, threads per core
- `$ mpstat -P number` Displays CPU statistics of single core processor indicated by number which can vary from 0 (means 1st core) to n if the CPU has more than 1 core.
- `$ mpstat -P ALL` Displays CPU statistics of all core processors.

### 6.2 Monitoring Memory

To check the current RAM installed on the system we can run the following commands:

- `$ free -h` Displays memory information such as total, used, buffer/cache and available RAM. Also it shows information about SWAP disk/file available and used space
- `$ vmstat` Displays information about the virtual system memory including memory information, cpu utilization, swap usage, and input/output operations related to block devices such a disk
- `$ vmstat f` Displays the number of processes created since the last boot of the system. Including foreground and background processes.

### 6.3 Monitoring Storage

To check the current storage usage on system we can run the following commands:

- `$ df -hT` Display the current storage space in all exsisting partitions. Option -T display installed filesystem in partition
- `$du -sh path` Shows the current space occupied by path in partition
- `$ lsof` Displays the information of current system files opened by daemons, users or processes.
- `$ lsof -c sshd` Display all the system files opened by command sshd
- `$ iostat -c 4` Monitors and displays system input/output device load information from devices and partitions, the -c option specifies how often to update the output

### 6.4 Logs

Logs are normaly binary files that record information about system activities and events. Logs are located in /var/log and store detailed information about proccesses, errors, warnings and other important information that is very useful for system troubleshooting and monitoring. In modern linux distros the logging is performed by the subsystem systemd-journald. The journald is a centralized subsystem that records and mantains structured and indexed binary files for loggin data, the information usually comes from different sources like the kernel, daemons, services, processes, applications and so on. The utility to navigate and search through these binary log files is journalctl

- `$ journaltcl -t docker -p err -b -x` The command displays all error messages (-p) adding explanatory ouput (-x) since the last boot (-b) cointaning the tag or syslog identifir docker (-t)

Another important concept is log rotation, this process allows for control of the size and number of log files generated by a service or application. As logs are continously generated over time they can consume a lof of disk space, log rotation manages the policy for compressing, archiving and deleting old log files and thus ensuring the log space to not become cluttered.

Log rotation includes:
- Rotation policy: Determines the maximum size and the maximum number of log files to keep as well as the frequency of the rotation (eg: daily, weekly...)
- Compressing and Archiving: When log file reaches the maximun size  allowed it is compressed and archived using tools like tar and gzip and saved.
- Creating a new log file: After archiving old log file a new one is created to continue the logging of the service or application
- Deleting old log files: When the number of log files reaches the maximum allowed, old log files are deleted.

Log rotation configuration files are unique for each service and are located under **/etc/lograte.d/**. If we would like to create a new log configuration for an exsisting service we need to create the corresponding file containing the configuration in this folder.

- `$ sudo vi /etc/logrotate.d/example` Creates example configuration file
- Configures the log file(s) to be rotated, in this case /path/to/example.log and configures the policy for rotation
  ```
    /path/to/example.log {
    rotate 7 # Maximum number of files to be keep
    size 100M #Maximum size for each log file to be rotated
    daily # The rotation policy
    missingok # If log file is missing don´t report an error
    notifempty # Do not rotate if log is empty
    compress # Compressed rotated logs
    delaycompress # Delay compression of rotated logs until next rotation cycle 
    create 0644 user group # Create new log file with owner "user" and group "group"
    }
```
- `$ sudo logrotate -d /etc/logrorate.d/example ` Checks log rotation configuration policy
- `$ sudo logrotate /etc/logrotate.d/example` Applies log rotation configuration policy and creates rotation if neede

Logrotate is automatically scheduled to be run daily by as a cron job. The configuration can be found in **/etc/cron.daily/logrorate**



