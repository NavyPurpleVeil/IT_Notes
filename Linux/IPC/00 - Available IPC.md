IPC - InterProcess Communication
---------------------------------

InterProcess Communication are operating system features which enable communication between a pair of processes.

IPCs are divided into ones provided by the operating system.

And abstraction layers that build upon the IPCs provided by the OS.


------


Signals
--------

One of the most basic ways processes can communicate with each other is via signals. Signals usually are used by the operating system to communicate things to the process, there are a couple of UserDefined signals.

List of signals and their default actions on linux can be looked up in the man page signal(7)

Most notable one(imho):

SIGALRM Timer based alarm
SIGHUP Terminal hangup
SIGCHLD Child has died

SIGPIPE Pipe has no readers

SIGTERM Gently terminate the program
SIGKILL Unmaskable termination

SIGSEGV Access to the invalid memory
SIGBUS Unaligned memory access

SIGUSR1 User Defined 1
SIGUSR2 UD 2

---------

Named Pipe
-----------
Named pipes are special files, they allow a 1 way communication with another process[one which reads and one which writes].
Named pipes generally have a 64-4KiB buffer(for now, there's a define that you can use to get the current platforms named pipe bufffer size).

Named pipes signal via poll(2) readiness to read, as well as closure of the pipe.
Named pipes can be reopened for write by another process[while your read end is still open], but it won't clear the poll(2) POLLHUP status.

-----------

Shared Memory
-------

Shared memory allows more than one processes share memory with other processes.
Shared memory is different from sharing a linked library memory in that it isn't write protected.

This way programs can exchange data without having to utilize syscalls of the operating system.
Shared memory allows much higher throuput to be achived, than a UNIX socket data exchange.
That's because shared memory features a lot less context switches, the only context switch that shared memory defacto required for InterProcess Communication is context switch to the reader program.  

-------

Message Queues
------

Message Queues can be used to exchange small bits of data, message queues just like shared memory exist within unaccsible private system path.



------

Unix Sockets
------
Unix sockets are sockets which enable datagram communication between processes.
This datagram communication, also allows prototyping network communication, as the apis used for communication are the same.
The main difference between UDP and Unix Socket is that the datapacket in case of UDP can get lost.
Just like with UDP send order isn't guranteed to be preserved.

One process can communicate to multiple other processes?
Or just one to one?
Crucial.

------

Listenning on network/localhost
------
Usually used when we want to RPC(Remote Procedure Call) over networks.
Can be done over TCP/UDP(and any custom protocol implemented on top of UDP).
Usually some form of:
RPC-JSON RPC-XML api-http server custom protocols

------


Userspace-defined IPC
------
One of the most common user-space defined IPC interfaces is D-BUS, which runs on UNIX sockets.
D-BUS is a form of local Procedure Call IPC in which programs can register the functions other programs can use.

------
