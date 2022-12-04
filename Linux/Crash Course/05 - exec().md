How are programs started on Linux?
----------------------------------
At first a process is fork(3)ed.
After which the process is setup, the setup includes closing all possible IPC methods or setting them up in correct way, as well as possibly setting prctl(2)s in the child process.
After which exec(3) is called to start a new process.

fork(3) under the hood uses clone(2), which is also the basis of posix threads implementation.
(Kernel term for process)Thread group is a (Kernel)thread who owns other (Userspace+Kernel)threads of a (Userspace concept)process.

After fork the program control flow starts from the return from fork.

What's prctl()?
---------------
Process controls, there's a lot of one of the better known uses for prctl is setting up userspace syscall emulation.

Neither exec() doesn't do file descriptor clean up, it's important for a program using exec() to close any dangling file descriptor to avoid leaking user data or open network sockets etc.


What's fork()?
--------------
Fork starts a new process, fork can either fail or suceed, when it suceeds it "suceeds" in 2 threads which return 2 different things.

parent receives the id of the child.
0 is returned in the child.
-1 on error.
Your program needs to handle both success cases at once, as it doesn't start the process fresh and mearly copies it and resumes execution from the same position.

What are the different exec()'s?
--------------------------------
In a nutshell, exec*e contaisn $env variables, the rest mearly copy the parents enivormental variables via a global external varriable environ.
execl treats each argument as a new string/new argument to be provided to a function via varriable length arguments, while execv treats it as a pointer to an array.



