
                  
                  Feb 15th, 2026

Topic :- Understanding linux permissions and dir access

Environment:- 

OS- Redhat linux
VMbox
User- Root and jogiyat

Commands:-

 chown
 chmod
 umask
 .bashrc
 .bash_profile
 /etc/bashrc
 /etc/profile
 source
 setfacl -m u : sahar : 755 apple.txt
 setfacl -m g : msoft : 750 /root/opt
 getfacl
 locate
 find
 
Error/issues

cd: /root/apple: Permission denied

Why This Happened (Root Cause)

I changed the permissions for apple directory but not for /root and tried to access with jogiyat(normal user). Thus, permission denied. Normal users cannot access /root directory. Linux checks permissions from parent directory to child.

How I Fixed It

Created directory in /opt
Changed ownership to normal user
Used correct permissions instead of 777

Commands Used to Fix

mkdir /opt/apple
chown jogiyat:jogiyat /opt/apple
chmod 755 /opt/apple



 What I Learned (IMPORTANT)

- /root is accessible only to root
- Parent directory permissions matter
- chmod 777 is not a real solution
- /opt is used for shared applications

 If I Had to Do It Again

Create shared directories outside /root from the beginning

Related Concepts to Revise Later

Linux directory traversal
File permissions (700, 755, 644)
chown vs chmod
—--------------------------------------------------------------------------------------------------------------------------

                      
                Feb 16th,2026

Topic :- Find and wildcards (*,?,[ ],{ },)						

Environment:- 

OS- Redhat linux
VMbox
User- Root 
		
Commands:
find
man find
zip 
ls -t | head -n1   ================> find the latest file
xargs ========================> (Converts input to arguments: While the pipe (|) operator passes the standard output of one command as the standard input of another, many commands do not accept input this way. xargs solves this by building and executing commands using the input data as command-line arguments.
Handles large argument lists: One primary use is to efficiently manage long lists of input (e.g., thousands of filenames from a find command) that might exceed the operating system's maximum command-line length limit. xargs automatically splits the input into smaller batches to run the target command multiple times, as needed.)
uname========================> kernel info
cat /etc/os-release }
hostnamectl           }    both used to find out installed server version
ls -i  ================> to find inode
cd -  ================> to go to most previous working dir
cp
rm -r * ===============> deletes all file/dir form pwd
head 
tail
wc ==================> count words in a file
sort 
cut 
chage -d 
passwd -e =============>to force user change the password on first login
last ==================> logins details
lastb=================> failed logins
usermod -g existing-grp username====> change user primary group
id username ===========> to check UID and GID
tr ====================> translate, delete and squeeze texts in a file
grep (global regular expression print)==> to search patterns (words) in files


(‘*’,?,{},[],() wildcard)
cp -rv a*.txt  /backup======> copy all txt files start with ‘a’ to backup directory


(VI/VIM editor specific)-
:! ===================> return to shell without leaving VI/VIM editor/run shell commands
co (i.e 10co100)========>copy 10 lines and paste after 100th line
vim -o (file1 file2 file3) ===>open multiple files in Vi editor (ctrl+ww—-> to switch between them )
:e ===================> to read /edit another file
Up arrow=====> to go to beginning of the line
$ ==========> to go to end of the line


Error/issues
Couldn’t change permission

Why This Happened (Root Cause)
1. Used ‘chown’ command instead of ‘chmod’
2. Used ‘chmod’ with wrong syntax 

How I Fixed It
Used ‘chown 755 /opt to change it from rwx permission to 755
Also, the special permissions were set (SUID & SGID) that made permissions look like drwsr-sr-x. 
Commands Used to Fix
Used chmod u-s /opt and chmod g-s /opt respectively to change it to execute mode x.

Related Concepts to Revise Later

Ownership
Permissions
SUID &SGID
Stickybits 

                    
                    
                    Feb 21,2026

Topic :- Disk management 

Environment:-  VM CentOs

Commands:- 
lsblk =========> list block device
dh -h
dh -i
fdisk =========> create,edit partitions 
fdisk -l 
Partprobe, partx, kpartx ======> updates new partition table info to kernel
mkfs -t <ftype> ====> create file system
lsblk -f ===========> list filesystem
mount============> list mount devices 
 mount -a


What I Learned today
Learned basics about disk management
Add virtual disk in VMBox (VM manager)
lsblk command to list disks

Cmd       device_name
fdisk      /dev/sdb 
 
To access newly created partitions and store data
1 make filesystem
2 create mount points(directories) and assign mount points

Making filesystem
mkfs -t fstype /dev/<device-name>
mkfs.fstype /dev/<device-name>



Linux uses ext2/3/4 and xfs file types
Mounting temporarily
mount <option> /dev/<device-name>  /<mount-point>
Mounting permanently
edit /etc/fstab
Then add new filesystem or device info into it
<devicename>  <mount-point>  fstype   permission   dump  fsck

Wrong file type in “fstab” page can send your system/vm into maintenance mode.



										Feb 22,2026

Title:- SWAP

Commands:- 
free ========>to print memory size
free -h=======>to print memory size in human readable format
       -s=======> to repeat command every N seconds (ie -s 3)
                           ctrl+ c to stop
       -c=======> t repeat command for N times (-c 5)
swapon -s====>to check swap status
swapoff
du =======> to estimate file usage
cat  /proc/sys/vm/swappiness ===> to check swappiness value
What I Learned today
How to increase Swap partitions (2 ways)
By creating dedicated swap partition(requires free space on avai storage device)
=> fdisk /dev/sdb
create new partition
Change type to swap
Make file system by mkswap /dev/sdbN
Enable it by swapon /dev/sdbN (if did’t update itself)
Make permanent by vim /etc/fstab
To disable swapoff /dev/sdbN
By creating swap file
=> dd if=/dev/zero of=/swapfile bs=1M count=1024
Here; 
dd= data duplicator
if = input file
of = output file
bs = block size (KB,MB,GB)
count = to multiply N by bs

Swappiness :- 
Is a kernel parameter that controls the priority of system swaps data out of memory to the swap space.

To find out swappiness on a system
cat /proc/sys/vm/swappiness

EXTRA:- SWITCHING SYSTEMS (GUI TO TERMINAL ONLY)
CHECK SYSTEM FIRST
sudo systemctl get-default (Ubuntu/RHEL)

TEMPORARY SYSTEM CHANGE
sudo systemctl isolate multi-user.target
No reboot needed

PERMANENT SYSTEMCHANGE
sudo systemctl set-default multi-user.target
Reboot after 



										Feb 23,2026
TOPIC:- BACKUP and DELETING OF THE PARTITIONS(STANDARD PARTITION)
Always backup files/partitions before deleting.

Commands:-
STEP 1 COPY THE FILES/PART- 
cp 
dd
cat
tar
zip

STEP 2 UNMOUNT/SWAPOFF
umount 
swapoff

STEP 3 DELET fstab ENTRIES
vim


TOPIC:- LVM (Logical Volume Management)

COMMANDS-  for creating 
pvcreate ======> to create physical volumes
 Syntax 	pvcreate /dev/sd[N-N]
pvs ==========> to check physical volumes size/info
pvdisplay ======> detailed info

vgcreate =======> to create volume group
 Syntax	vgcreate <grp-name> <pv-name>
vgs ===========> volume groups info
lvcreate =======> to create logical volumes
 Syntax	lvcreate -L +<size> <grp-name> -n <lv-name>
lvs ===========> logical volume info
lvdisplay======> logical volumes detailed info  


Error/issues
Got error while making file system after creating LVM. 


Why This Happened (Root Cause)
Tried to format physical volume instead of Logical volume
Used mkfs.ext4 /dev/sdb (standard partition syntx)



How I Fixed It
Used mkfs.ext4 /dev/yellow(v_group)/clyde(logical_vol)

Commands Used to Fix


What I Learned (IMPORTANT)
Be mindful of what you formatting (standard vs lvm)


COMMANDS- for extending lvm
lvextend 
	lvextend -L +<size M/G> <lv_name>

To extend file system
resize2fs =====>for ext2/3/4
xfs_growfs====>for xfs

 Extending swap memory

Two ways for standard (discussed above)
Dedicated swap partition
Swap file system
If swap is an lvm;
Then add new physical volume into it

COMMANDS-
vgextend  <vg_name>  <pv_name>

Reducing LVM size
Note:- Never apply lvreduce on any LVM partition without file system resize.

Follow these steps before running lvreduce
Verify free space
df -Th
Stop volume
umonut
Check file system read error
e2fsck -f <lv_name>
Reduce file system
ext2/3/4 —-> resize2fs
xfs—--------> cannot reduce xfs filesystem
Then run 
lvreduce -L  <-size>  <lv_name>



REDUCINGE VOLUME GROUP SIZE
Reducing a volume group = remove a physical volume from it, 
to increase it = vice versa
lvremove  <vg_path>   <lv_name> (deleting logical volume)
vgreduce  <vg_path>  <pv_path>

TO DELETE VOLUME GROUPS
vgremove <vg_name>

TO DELETE  PHYSICAL VOLUMES
pvremove <pv_name>

TO RECOVER DELETED LVM PARTITIONS
Linux restores from “/etc/lvm/archive” configuration archive file

To list conf files
vgcfgrestore -l <vg-name>

To restore
vgcfgrestore -f <file-name> <vg_name>
example-
vgcfgrestore -f </etc/lvm/archive/force_0001-998609753.vg> <force>

To access that file, you  need to activate it by
lvchange -ay /dev/force
mount 
Then access 


									Feb 24,2026
TOPIC:- LVM SNAPSHOT
lvcreate -L +size -s -n <snapshot_name> <dev/vg_name/lv_name>

Configuration file for LVM snapshots- /etc/lvm/lvm.conf
Search for /autoextend → make changes if needed

To restore or merge lvm snapshot volume
Unmount the source volume
Run
lvconvert --merge  /dev/vg_name/lv_snapshot
Deactivate and activate the volume
lvcahnge -an /dev/force/jogs_snapshot
lvcahnge -ay /dev/force/jogs_snapshot
mount 
Once the snapshot volume process completed, snapshot volume will be deleted.


LVM MIGRATION
To migrate data from one pv to another pv.
vpmove   <old_pv>  <new_pv>(move all lvs)
Vpmove -b -n <specific_lv> <old_pv>  <new_pv> (move specific lv)



Reduce the size of a partition formatted with xfs filesystem.
Not possible with xfs_growfs command

Step-by-step process-
Backup the data using xfsdump
Unmount the file system
Shrink the volume to desired size using lvreduce
Format the lvm partition
Remount the file system
Restore data using xfsrestore



										Feb 25,2026

SYSTEM WENT INTO MAINTENANCE MODE AFTER REBOOT



WHY THIS HAPPENED??!!
Installed GUI on CentOS Stream VM, later switched to multi-user (CLI) target. After reboot, system entered emergency mode due to vmwgfx driver mismatch in VirtualBox. Learned that changing target does not remove GUI drivers and that kernel/graphics compatibility can cause delayed boot failures.




Topic :- PROCESS MANAGEMENT

A process is a running instance of a program
A process normally runs in foreground. Enter “&” sign at the end of the command to run that process in background.

To send a running process in bg; press ctrl+z(this will stop process) and then type bg(will continue in bg)
To get process back to foreground, run fg<process_ID>
COMMANDS;-
echo $$ =====> current shell process ID
echo $PPID===> checks parent process ID
ps ==========>current process status
ps -ef========> every process details
ps -aux
	-a = all
	-u= users
	-x= all the process that are not associated with any tty
fg <process_id>===> get process back to foreground
bg 
jobs ======> to check running processes in background
sleep =====> to delay a process for specific amount of time
pgrep <process name>
pidof <process name>=======>both comm to find process id of a prcs
kill
killall <process_name>  
pkill 
nice -n <niceness value> <process_name>
renice -n <niceness value> -p <PID>




Daemon :- this process starts at the system startup and keep running forever. These daemons never die.

Zombie :- when a ps is killed but still shows up in process table. You can’t kill zombies coz they are already dead.

Orphan ps:- whose parent ps is no more exist. Systemd takes care of such ps.

NICE :- start a ps with given priority
	Lower niceness= high priority & more CPU time
	Higher niceness= lower priority & less CPU time
	Range = -20 to 19
	Default = 0
nice -n <niceness value> <process_name>

RENICE:- Change priority of already running process
renice -n <niceness value> -p <PID>


Systemctl
systemctl <action> <process_name>





TOPIC;- PACKAGE MANAGEMENT 
To install packages
yum, dnf- for RHEL, CentOs
Apt- Ubuntu ect.

(RHEL):-
“.rpm” package list
To remove packages
yum remove <packagename>
yum erase              ‘’

To check for system update
yum check-update

yum repolist======> list repositories 

UBUNTU/DEBIAN
“.deb” package file
apt update
apt upgrade
apt list

~/package-commands.md

## Ubuntu (apt)
sudo apt update          # Refresh package list
sudo apt upgrade         # Upgrade everything
sudo apt install [pkg]   # Install
sudo apt remove [pkg]    # Remove (keeps config)
sudo apt purge [pkg]     # Remove (including config)
apt search [pkg]         # Search
apt show [pkg]           # Show details



## RHEL (yum/dnf)
sudo yum check-update    # Check for updates
sudo yum update          # Update everything
sudo yum install [pkg]   # Install
sudo yum remove [pkg]    # Remove
sudo yum search [pkg]    # Search
yum info [pkg]           # Show details
yum provides [file]      # Find which package owns file


 "Initial commit—my Linux notes"
