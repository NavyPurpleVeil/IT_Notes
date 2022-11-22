Virtual Domains
---

Virtual domains are a way for a single web server application(often listenning on a single ip address tied to multiple domain names), to handle multiple different domain names with a different filesystem namespace(not to be mistaken with a linux namespace feature).

A single web server can those handle multiple websites whose domain differ and whose files shouldn't be mixed up(as logn as our backend and database is properly setup etc.); 

--- 