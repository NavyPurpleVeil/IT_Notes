What is kernel?
---------------
It's a central part of an operating system which enables the
Kernels manage hardware and are at a core of software interactions.
We're only going to talk about monolithic kernels in which the filesystem, ipc and more occurs in a kernel rather than userspace.
Kernel is by and large responsible for distributing hardware resources to software.
This is done through process schedulers(which process gets to run), io schedulers(which write/reaad request to complete), niceness(priorities), cgroups(resource restriction and management), in part even namespaces(containarization), tcp congestion algorithms(tcp packets congestion control) etc.

Internal kernel apis are like layers of an onion, a layer of abstractions a standard interfaces which drivers implement.
For example operations on a file systems implement a standard Virtual File System interface.

What is a initramfs?
--------------------
It's an initial ramdisk, it's the initial file system which is used by the OS.
Contains drivers and basic utilities to aid with loading up the true operating system.
There are some basic utilities to aid recovery, but not all usefull tools might be available by default.
Because ramdisks contain kernel modules and firmwares, one is required per every kernel(size of a ramdisk also modifies the boot time).

Ramdisks can be compressed with multiple algorithms, this decompression can be so fast that the actual bottleneck is retriving the ramdisk(this isn't true for lzma).

What is init?
-------------
It's a process with PID 1, this process cannot be killed nor crash without causing a kernel to crash as well.
Without PID 1(process with no parents before itself) it simply isn't possible to rip any other process.

How does a linux system start?
------------------------------
First a bootloader pulls up an initiramfs.
An init system is run from the initramfs, short setup of drivers is done, and the rootfs partition gets(system can run off of said ramdisk).
The init runs a service manager and plenty more services to then start up the system as seen by the user.
On desktop includes things like network managers, dhcp clients, any servers, dbus, udev, display manager(GUI login prompt), spawns some form of getty etc.

Linux kernel can also take a role of an efi binary(EFISTUB) and those together with kexec(kernel exec) be used as a bootloader.



