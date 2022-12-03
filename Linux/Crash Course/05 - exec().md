How are programs started on Linux?
----------------------------------
At first a process is fork(3)ed.
After which the process is setup, the setup includes closing all possible IPC methods or setting them up in correct way, as well as possibly setting prctl(2)s in the child process.
After which exec(2) is called to start a new process.

fork(3) under the hood uses clone(2), which is also the basis of posix threads implementation.
(Kernel term for process)Thread group is a (Kernel)thread who owns other (Userspace+Kernel)threads of a (Userspace concept)process.

After fork the program control flow starts from the return from fork.

What's prctl()?
---------------
Process controls, there's a lot of one of the better known uses for prctl is setting up userspace syscall emulation.

Neither exec() doesn't do file descriptor clean up, it's important for a program using exec() to close any dangling file descriptor to avoid leaking user data or open network sockets etc.
