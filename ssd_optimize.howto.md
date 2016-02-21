SSD optimization for Linux
==========================

###Alignment

Tools like 'fdisk', 'gparted', 'parted', etc now handle partition alignment automatically. To check if a partition is aligned, the following command should return '0'.
```
 $ sudo blockdev --getalignoff /dev/<partition> #e.g. sda1
```
It is important to have an aligned partition, because SSDs have a limited amount of writes. An SSD Block consists of 512KB. Because of the way SSDs work, changing even 1 bit on a Block forces the whole block to undergo a read, modify and write cycle. If the partition is not aligned, 2 Blocks will go through the cycle, thus reducing the performance and lifetime of the device. A Block is a combination of 4KB parts called 'Pages'. By aligning the partition to these Pages, we ensure that the SSD doesn't make any unnecessary writes.

-------

###I/O Scheduler

Switch CFQ scheduler (Complete Fair Queuing) to NOOP or Deadline for better performance. Since the seek times are identical for all SSD sectors (unlike in HDDs) there is no need to re-order I/O queues. Verify the currently used one:
```
 $ cat /sys/block/sdX/queue/scheduler

   noop deadline [cfq]
```
NOOP is the simplest I/O scheduler for the Linux kernel. All incoming requests are inserted into a simple first-in-first-out (FIFO) queue. Deadline is smarter by imposing a deadline on all I/O operations to prevent starvation of requests. Read queues are given higher priority as read requests have an expiration time of 500ms, while write requests expire in 5 seconds, by default.

The one currently in use is the one between brakets. Switch with the command:
```bash
 # echo deadline > /sys/block/sdX/queue/scheduler
 $ cat /sys/block/sdX/queue/scheduler
   noop [deadline] cfq
```
This is not permanent. Changes will be reset on reboot

Although the previous method will work, it is best to let udev handle the scheduler assignment. 
>/etc/udev/rules.d/60-schedulers.rules

>> \#set deadline scheduler for non-rotating disks
>>ACTION=="add|change", KERNEL=="sd[a-z]", ATTR{queue/rotational}=="0", ATTR{queue/scheduler}="deadline"

The changes should take place on the next boot. Additionally, the `deadline` scheduler will only be applied for non-rotational drives.

------------

###TRIM

To verify if your SSD has TRIM support, download `hdparm` and run the command:
```
 $ sudo hdparm -I /dev/sda | grep TRIM #substitute 'sda' with your corresponding SSD drive
```
As of Linux kernel version 3.8 onwards, the filesystems that support TRIM are Ext4, Btrfs, VFAT, JFS, XFS, F2FS. If you enable TRIM on a Ext3 filesystem, it will be booted as read-only.

Check trim related activities:
```
 $ journalctl -u fstrim
 $ systemctl status fstrim
```
When you delete a file, the sectors on the drive are not changed. What is changed is the internal mapper of the drive to the file, which is deleted. When the drive is rewriting those sectors, it first deletes the information and then writes on the sectors. By performing trim, the system basically is told to overwrite whatever is marked as empty, thus making only one write operation and extending the lifetime of the drive.
```
 $ sudo fstrim -v /  # single trim operation
```
For most desktop systems the sufficient trimming frequency is once a week. 

### `noatime` mount option

Using the `noatime` flag option in fstab, eliminates the need by the system to make writes to the file system for files which are simply being read. 

	WARNING: BACKUP THE FILE FIRST!!!


> /etc/fstab
  >> /dev/sda1   /      ext4   defaults,noatime   0 1  
  >> /dev/sda2   /home  ext4   defaults,noatime   0 2  
 


>NOTE: This will cause issues with applications such as Mutt, as the access time of the file will eventually be previous than the modification time. To ensure this doesn't happen to the atime field, use 'relatime' instead, although it is not as effective.

.
>NOTE: By adding the 'discard' flag to the fstab file, TRIM is essensialy being enabled, but it is discouraged as it will frequently "trim" unused blocks and it might negatively affect the lifetime of poor-quality SSDs or sometimes even lead to partition table corruption. There is no need to use the flag if you use the fstrim tool regularly, maybe as a cron weekly job.

-------

###Swap Space on SSDs

If you have a swap partition mounted on the SSD, you can use the command:
```
 $ sudo echo 0 > /proc/sys/vm/swappines
```
This will tell the system to only use the swap partition when the free memory space is depleated.


-------

###Sources
[ArchWiki: Maximizing Performance](https://wiki.archlinux.org/index.php/Maximizing_performance)  
[ArchWiki: Solid State Drives](https://wiki.archlinux.org/index.php/Solid_State_Drives)
