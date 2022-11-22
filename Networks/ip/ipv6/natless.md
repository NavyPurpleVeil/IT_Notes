If ipv6 is devoid of nat, are all non-ipv4 compatibility all ipv6 networks public? If not what alternative solutions does ipv6 provide in place of NAT?
---
The external in traffic needs to be let through.

An open receive(listenning port open for the 2-way UDP/TCP communication) port in the OS can't be told apart from a regular one.

it can't be;

---