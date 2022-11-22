What is a cpu?
--------------
Cpu is a processing unit which executes instructions in a given instruction set.
Some cpus emulate different instruction sets and use a different one beneath the surface.

Instruction set
---------------
An instruction set can be thought of as an language in which a cpu understands and executes instructions in.

ALU
------------
Arithmethic unit.
Different instructions can have different levels of parallerism, some of them need to be delayed to be executed again.

Control unit
------------
Schedules the instructions, decodes and manages the available resources of a cpu.

Registers
---------
They are just units of data, that directly on a cpu die, ready to be accessed immiedietly.
Registers can have their own specific purposes, as well as tie to different data types in a higher level language.

Addressing modes
----------------
Cpus can address the data to access with different levels of addressing.
Memory address that can point to another memory, or a register that can point to a different point in memory, all with various degree of this kind of "layering". 

MMU
----------------
Memory mapping unit is concerned with mapping the virtual addresses to the physical addresses in ram.
Application programmers access an abstracted MMU. 

DMA
----------------
Direct Memory Access, direct access to user memory, free from having to interface with the cpu.
In consoles like Nintendo DS DMA let's the user schedule quick memory to memory transfers from a cpu layer.

Memory controller
-----------------
Keeps track of the timings as well as interfaces with the memory interface.

Retirement unit
---------------
In speculative execution, retirement unit deals with "updating" the cpu+memory state accordingly.
Retirement unit itself can be speculative, in which case the cpu takes a deep penalty to "goes back in time" to fix up it's mistakes.

(Speculation:)
Likely during the context switch/interrupts, the state needs to be known before executing the interrupt, this adds to the context switch latency.

Preventing stalls
------------------
By preventing stalls and using a seperate bus for communication with different components, cpus got a lot faster.

What is a stall?
----------------
A stall occurs when the cpu can't execute instructions.
This is different from the idle states, as in case of a stall cpu has further instructions it can execute, and in case of an idle state, the cpu awaits an interrupt(usually a timer interrupt of some kind).


What is cpu pipelineing 
-----------------------
Cpu pipeline is scheduling another instruction.
Cpu pipelines can have an length in X instructions,
as well as be of varried lengths depending on the instruction.

The easiest cpu pipeline to understand is an:
Load-Decode-Execute pipeline

What is SETcc/CMOVcc
-----------------------
Branchless instructions read the flag register
they avoid pipeline resets and just work like a NOP on failure
SETcc/CMOVcc doesn't reset the flag register.

What is superscalar
-------------------
This is a type of instruction scheduling in which multiple instructions
are executed at once.
The types of instructions that can be scheduled on these independent units defines the superscallar.
```
      [L][D][E]
      [L][D][E]
   [L][D][E]
   [L][D][E]
[L][D][E]
[L][D][E]
```

What is branch prediction?
--------------------------
The cpu has an internal state in which it assumes it took the right branch, before it executes the branch instruction.
Once it takes the wrong branch whole cpu pipeline needs to get flushed alongside the internal state.

Uncertain:
* The branch which isn't taken is flushed before execution.
* The branch which isn't taken isn't scheduled anylonger onto the cpu and is executed on the seperate internal state.

Complicated and not in use:
* The cpu is "split in 1/X" and the branch which isn't to be taken(if cpu isn't certain) is taken anyway, only part of the cpu gets flushed.
The cpu doesn't know a real chance for a branch to hit, so this can average out the worst outcomes.
But this also cuts any gains on the branches that cpu can hit on reliably.

What is a microarchitecture?
----------------------------
Rather than executing the raw instruction set, a microarchitecture allows the cpu to better fill-in their available units, the instructions gets decoded into micro/macro operations.

What is a calling convention?
-----------------------------
A calling convention is a convention for where and how arguments of a function return to the caller. 


What is out of order execution?
-------------------------------
Out of order execution partially prevents stalls that are induced by avoiding over-optimization to a specific cpu architecture(Pentium 1 manual).
Instructions are executed out of order, while the cpu resource is busy or memory can't be immiedietly accessed.


What is a ring?
---------------
Rings are a security context, not all registers are available to the programmer of an application on every specific ring.
Usually chainging the ring also modifies the ability to modify the MMU and much more.


What is a context switch?
--------------------------
Context switch occurs when a cpu switches from one RING(security domain) to another.
Context switches are also descriptions of cpu switching from process to the next.

What is endianness?
-------------------
Endianess describes how the smallest word(usually 8Bits) of the cpu are arranged in memory with higher size words(2-XXBytes).
There are 2 orders that we can distinguish between:
- Little Endian (This is what most architectures use nowdays)
- Big Endian (Also known as Network Order)

With Little endian the bytes end with what in the decimal would be the BIG values. (yeah Little Endian starts with the bytes that are the smallest).
This matters when you cast a pointer from a bigger data type to a smaller one, in Little Endian you'll be reading completely different numbers when traversing sequntially.


What is a syscall?
------------------
Syscall is "system call"(synomous with "system function call"), it's a kind of interrupt, which puts a cpu into Ring 0.
Syscalls have a number, and a jump table(the number can be thought of as an index in the table, and the contents as a pointer to a kernel function that gets executed).