Why cover file systems first?
-----------------------------
It might feel counter untuitive, afterall filesystems are a basis of the most popular operating system, that being windows.
But it's what most people get wrong and missunderstand the most about.
It's the number one barrier of anyone who touches their computer casually to reinstall a system as well as install another one.

What is a file system?
----------------------
File system is a database which holds the files, folders and any odities of the operating systems that users don't usually deal with.
This database exists on your drive in a space that was allocated for it.
It's also a set of features that a system implements to handle said database.
This implementation as well as the way data is laid out can cause fragmentation to happen more often than in another file system.

Where is the file system at?
----------------------------
Unless you touch raid, lvm2, appfs you deal with a classic meal like mac and cheese:
* Partition table exists on the disk.
* The partition table describes where partitions are on the disk
* But the partition is genuenly just the description of what file system the partition has, how far from the beginning of the disk it is and how much space it takes.

As such the specific file system that you might be referring to might be called a partition.
Yet it's also called a disk, why so?
Because it really is just a partition which spans pretty much the entire drive(together with the partition table etc.).

Often times there are hidden system partitions that you don't want the user to modify or tamper with, these might be hidden from the user as such even if you might see just C: drive you actually have more of them in your computer.

raidlvmappfs what?
------------------
Non crucial for home use.

raid is just an array of at least 2 or more disks, software raid might be a raid between partitions rather than the disks themselves.
Raid is a way to speed up or provide redundancy(reduce amount of space that a drive can carry) to your data.

lvm is a file system type that's essentialy a tool to describe partitions in greater detail.
What kind of detail?
* Say you want a partition spanning 2-3 drives.
* Say you want to accelerate your hdd with a TB nvme ssd as cache, but you want to just see the 14TB hdd of yours.
* Maybe for whatever reason you can't decide if you want to dedicate 200 or 300GB of your ssd(1000GB) to C: and 800GB to D: drive.
LVM is the man to do the job.

With LVM you can overprovission.
Over-provissioning is like a white lie, but a lie that can turn very dark if left unchecked.
LVM can lie to the file system that it has more space than there's logically available.
This way you can have a 800GB D: partition and 300GB C: partition in a 1000GB drive(notice how we've over-given 100GBs). So long as you don't fill up the partitions you are good to go.

The reason this works is, because of "trim", "discard" the file system(say ext4, which is the recommended file system to use) notifies the block devices(will explain what it is):
* Upon removing a file file system notifies a LVM block device with a discard command.
* LVM then knows this space is unused, and now if another file system needs the space it can provision it to another file system.
* LVM then notifies the drives that owned the block of data that got freed up and they do their magic.
Without LVM discard might go straight to the drives.

APFS is similar tech to LVM but for macOS.


Features of file systems
------------------------
Filesystems can have plethora of features, and I'm going to talk about killer features only that I can recall from memory.
* Journalling (preventing the data loss of ext2 and fat32)
* Preallocating files (allows to create a huge file in mear nanoseconds to seconds, without having to write any data to it)
* Transparent compression (files can be compressed and programs don't get to see the compressed data)
* Online deduplication (files can be deduplicated on a file block and file level)
* Online resize (while the system is running the file system can be made bigger or also smaller)
* CoW (Copy on Write, we can instantly copy a file, and only once we modify it do we actually do the copying, the copying might occur at a file block level acting as a semi-deduplication)



What is a mountpoint?
---------------------
Now we need to mount the filesystem somewhere.
On windows we assign letters C: D: E: etc.
And likewise a file might exist on a path: C:\Windows\System32\cmd.exe

On linux and conversly macos they exist on the filesystem path.
Which basically means there exists a master path, called rootfs(root file system) "/", this file system can't be unmounted, but it can be modified at runtime(outside of system startup it usually isn't).
This basically means anywhere there's a folder on Linux, there's a parking spot for your file system.

Some trivia:
pivot_root(2) is a syscall used to modify where the outlook of a "/" is.
It works like chroot(2) except you can't reverse it's effects.


What is a bind mount?
---------------------
Just imagine you mount a directory into another directory.
You can also then unmount the original mountpoint, and the bind mount is still going to be usuable as is, because the system keeps track of such references.

Recursive bind mount let's you bind mount a directory together with bind mounts it contains.


How does it differ to a link?
-----------------------------
A (sym)link is simply a special file stored on the file system, this file just points to another relative(``./relative/folder``) or absolute(``/absolute/location``).
It's different from a bind mount in that it can just be moved around freely, and it isn't a file system reference.

Now how does a symlink differ from a hardlink.
Symlinks store the location.
A hardlink stores a reference to a file that exists only in this specific file system. The file system keeps track of references to the file through an inode identifier.
Using a file through modifying a bind mount, link, hardlink is the exact same thing as using the original file.

```bash
$ ln -sf jellyfish-400-mbps-4k-uhd-hevc-10bit.mkv jelly #symlink
$ ln -Pf jellyfish-400-mbps-4k-uhd-hevc-10bit.mkv fish #hardlink
$ ls -l jelly jellyfish-400-mbps-4k-uhd-hevc-10bit.mkv fish #list files
-rw-r--r-- 2 1001 1001 1504953150 wrz 10 20:10 fish
lrwxrwxrwx 1 1001 1001         40 lis 30 16:30 jelly -> jellyfish-400-mbps-4k-uhd-hevc-10bit.mkv
-rw-r--r-- 2 1001 1001 1504953150 wrz 10 20:10 jellyfish-400-mbps-4k-uhd-hevc-10bit.mkv
```

Where are the mountpoints described in?
---------------------------------------
/etc/fstab describes automatically mounted partitions.
You can cheat and mount a partition to then find out what to insert into /etc/fstab by reading /etc/mtab file, which contain currently mounted partitions with their respective mount options.

You can also look up mounted partitions with the following commands.
```bash
$ mount
$ lsblk
```

Filesystems in userspace
------------------------
Filesystems can be implemented in userspace, to what otherwise could have been not just risky code, but one which doesn't need to get reimplemented if the internal kernel interfaces change.
FUSE enables fast prototyping of custom filesystems, some of which complete network operations(for example there exists a filesystem for bittorrent and http downloads, as well as one for mounting the archive onto the system).


The permission system.
----------------------
Permission system restricts the programs run by users, it makes it possible for one user files to be inaccssible by the other.
There exist 2 types of permission systems in Linux:
- Discrete Access Control (this usually restricts the user)
- Mandatory Access Control (this usually restricts the specific binaries)
Mandatory Access Control is implemented through Linux Security Modules.

Discrete access control is a well known
special,read,write,execute user,group,other system;
```bash
$ ls -l
-rw-r--r-- 2 1001 1001 1504953150 wrz 10 20:10 jellyfish-400-mbps-4k-uhd-hevc-10bit.mkv
```
rwx means Read Write eXecute
Directories are marked with a capital "D". Links with "L". setuid/gid with an "s" near execute.
The discrete access is divided into owner, group and other(everyone).

There exists a special permission called Setuid and Setgid with which the program starts with permissions to execute setuid and setgid such program starts with the permission level of the user/group the file belongs to.
This is how sudo is able to be run by standard account user, yet appear as superuser.
There also does exist an Access Control list style permissions.

The discrete access control also has an "capabilities(7)", this allows a binary to run with certain permissions only afforded by the root user account, without compromising the entire system.
Some of those capabilities aren't safe.

Examples of mandatory access control systems on linux:
- Selinux
- Apparmor


