What is a memory?
--------------
You can think of memory as a lego road with houses attached to it.
You can rearrange the memory in any way you want.
And it can contain anything you want.

Now, that's what cpu MMU let you do, they let you tie physical address(managed by the ram stick+location on the motherboard).

On linux you can see this happen:
```bash
$ cat /proc/<pid>/maps
```
Maps contains MemoryMappings hence maps.

What is a rank?
---------------
Memory rank is a number of active chip-select pins on the DRAM stick.
It's like having another dram stick in one, by having one set of dram chips inactive, the other might be relatively free of idle time.

What is a channel?
------------------
Memory channels is a number of dram sticks, whose data can be accessed at once.
The data is interleaved between the dram sticks, this accelerates cache read-aheads.


What's bus width?
-----------------
Bus width usually describes the amount of data that is being read on the memory chips at once. The bigger the bus width the faster the bandwidth.