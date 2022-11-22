CGI
---
A simple fork() -> 
Put http request data in enivormental variables
dup2() the file descriptor of the client into stdout
-> exec()

The cgi program writes the http response directly into the tcp socket;

The env variables are prepended "http" to prevent arbitrary sensitive env variable injections;

The problem is that every connection requires at least 3 system calls( as well as a system call to sanitize other open file descriptors).
This eats up cpu time.

---

FastCGI
---

This is what php uses.


Fork() once, events back and forth are communicated over the unix sockets.

(Or the file descriptor might be comunicated with the open socket to the client via unix file descriptor sharing, in which case the fastCGI backend receives full control over the connection the same way CGI did).

The advantage is well we theoretically only fork()->exec()'d once;
No more no less, and all the communication happens over unix socket(which are faster than network loopback devices).

The program itself handles the communication in it's main loop;

---

A shared library tied to a web server.
---

In case of apache the exact threading model of how requests will get handled by the library gets choosen by the apache itself.

Introduce Apache MPM
---

MultiProcessing Module; Likely handles how apache handles requests;
PHP documentation suggests it doesn't just influence how the http requests get handled by apache.
(This suggestion is likely about the, php apache module, it likely ties into the requests, hence when PHP gets executed in the models different than preFork mt-unsafe functions get used by PHP).

Launching new threads and forking are expensive operations;
Re-using existing workers is ideal;

#### PreFork model:

[Parent] - [ Worker 1 ]
         |
         - [ Worker 2 ]

Apache forks itself ahead of time awaiting an request.
Once the request can be handled by an CGI application it setups the process and exec()s, and preforks a new worker.

Only a single worker can listen for new connections.
Parent doesn't really have a way to inject a file descriptor into the worker(pidfd sendfd requires ptrace capabilities: risk; unix sockets?).

The workers hold a lock in the shared memory;
Only one worker can take hold of the lock at once;
The one which does can listen for a connection.

#### Worker MPM:

					[ Listenner thread ]
[Parent] - [ Child ] - - - - |
		 |				[ Worker thread at least 2]
		 |
		 |			[ Listenner thread ]
		 - [ Child ] - - - - |
		 				[ Worker thread at least 2]

Similiar as prefork, the mutex changes hands, but one process can have more than one thread responding;
One worker thread one connection, worker threads handle keep-alive calculation;

#### Event MPM:

Similiar as worker, except the keep alive is handled by the listenner thread, which is eased up from using accept4(), with epoll() and kqueue() kernel features.

The worker thread responds to the client, one thread can those be used to respond to multiple clients, as we can watch on the response of multiple clients.

---

NGINX modules
---

Nginx is known to pre-spawn X worker process, each with Y worker threads;
As set in /etc/nginx/nginx.conf

---