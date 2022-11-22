C Interfaces
-----------------
C interfaces might be implemented for multiple reasons.
An example of where C interfaces are used is networking code.
The networking api can fit multiple types of networking transport layers, as well as physical layers.

A socket() call sets up an interface for the file descriptor.
Linux 5.15.79:net/socket.c:1452-1467
@1467 is where we actually initialize the socket structures;
We can see that when the operation fails, sock->ops is NULL (this structure contains protocol specific operations, which get used at the time of using another syscall).

Now every recvmsg, polls etc. can be handled by their respective syscalls with efficiency and elegance.

What enables creation of these interfaces?
Structs and function pointers.

Padded structs
--------------
One size fits all, but not all structs fit all.
```c
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
// base type is something along the lines of:
struct sockaddr {
  unsigned short int sa_family; // Declares type of address
  unsigned char sa_data[14]; // Fits any possible address type
};
// ipv4
struct sockaddr_in {
	__SOCKADDR_COMMON (sin_); // unsigned short int
	in_port_t sin_port;			/* Port number.  */
	struct in_addr sin_addr;		/* Internet address.  */

	/* Pad to size of `struct sockaddr'.  */
	unsigned char sin_zero[sizeof (struct sockaddr)
				- __SOCKADDR_COMMON_SIZE
				- sizeof (in_port_t)
				- sizeof (struct in_addr)];
};
```

Function pointers
-----------------
```C
// functionFactory is the symbol name
int (*functionFactory)(int, char**, char**) = main;

// Example interface for an in-game entity?
struct entityOps {
	void (*render)(); // renders entity on the screen
	void (*destroy)(struct entity); // clean up routine
};
```
