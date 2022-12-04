Asking for avability.
---------------------
There exist a set of functions which block your application until a desired event happens, those functions are:
* select
* poll
* epoll


select(3) is the most common interface available acrosss most major operating systems(Linux, BSD, OS X, Windows).
select on linux can't be used to probe file descriptors above the number 0-1023.
On windows it can't reach more than 1024 FDs.

poll(3)
Has no limitations of select(3), and has a much easier interface to work with compared to especially epoll(7) and select(3).
Can be used to poll any interfaces using file descriptors.

epoll(7)
native linux polling interface.


Asynchronous syscall execution.
-------------------------------
While there exists an interface for asynchronous IO we will skip on it.
It works well for network i/o, but hasn't been so usefull for local i/o.

io_using let's us execute multiple syscalls and receive their returns asynchronously.
io_uring is especially usefull in achieving high throughput with enterprise nvme drives.

All of this is made possible with the help of circular buffers, a data structure which allows for 2-way communication over a single array, the access to it is abstracted via liburing.
The interface isn't fully asynchronous as it requires the user to call a syscall upon adding one or more syscalls to the structure.
The response from the kernel is however asynchronous.

Userspace can also add a pointer that relates to any auxilary data we want to keep track of when the system sends us a return value of the syscall.
This let's to carry the state of our program and allows for a different unique shape of our application architecture, in which we deal with streams of succesfull/unsucessfull syscalls.
