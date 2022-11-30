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








