What is a cpu cache?
--------------------
Cpu cache, caches in recent reads and writes made by a processor.
As a systems programmer you need to understand the cache to write performant applications for the specific application.
Cpu caches(even those large 512MB ones) can't be mapped into the address space.

What are cpu cache levels?
--------------------------
Caches are divided into performance tiers.
Each cache level is slower, but usually bigger than the next.

L1C(ode) cache can contain decoded microOPs.
Level 1 cache works in the virtual addressing, rather than physical addressing and as such it doesn't utilize the TLBs.
(So part of the context switch time might be spent on updating the L1 cache addressing).

L2 and L3 are mapped into physical addresses.
Bigger caches have higher latencies, these latencies scale with physical on-die size.

What is a TLB?
--------------
TLB(Translation Lookahead Buffer), can be thought of as cache for MMU access, it's used on data accesses on L2, L3 and memory.

What are non-temporal writes?
-----------------------------
Cache reads and writes normally trigger a search on L1/[L2/L3+TLB], which can take time.
Said search can be avoided with non-temporal writes.

Non-temporal reads and writes are best used when you want to avoid polluting the cache, avoid said search and done all at once to the size of read/write combine buffer(usually some multiple of a cache line).

What is a cache line size?
--------------------------
Cache line size can be the smallest unit that can be written to and from cache.
By re-using smaller memory regions(and loading up data gradually).
Higher throuput can be achived.

What is associability?
----------------------
Associability is the smallest block of data that can be addressed by cache
Cache line * associability.
If your data structures can fit(this can include multiples of a data type) into the cpu cacheable data block, then access to your structure won't be possibly divided into 2 cache misses, but instead just 1.

Caches pre-fetch
----------------
Sequential reads of memory can work well with cpu's cache read-ahead.
Data can be preloaded into cache, when accesing sequential data this works in our favor, when loading heavily scrambled lists, the program and the cpu will work against each other.

Cache coherency
---------------
Each cpu often has it's own L1 and L2, when the task gets assigned to another core, Cache coherency protocols come to the rescue.
Cache coherency protocols also make it possible to share data in a multi-threaded program.
