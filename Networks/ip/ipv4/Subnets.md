What is a subnet and Why are subnets used?
---

Subnets definietly serve a purpose in breaking down a network into broadcast areas?

Computers will only answer to the broadcast address when it matches their subnet.
At the same time there exists a global 255.255.255.255 broadcast(that usually isn't let to cross NAT boundries).

---

How does a router and computers handle a subnet, it's outside vs within it's main network?
---

Inside a local network:
* The packets that cross subnet network boundries aren't let through, if the route can't be found. A gateway address is required to resolve subnet crossing [chuj wie].
* This behavior is similiar to VLANs, but instead of port/interface boundries the subnet address space boundries get respected.

Outside:
* Likely just tries to make it's packets pass through with another router.

---


How to find the subnet with a given subnet mask and ip?
---

``ip: 192.168.0.1``
``subnet mask:0xff 0xff 0xff 0x00``
``subnet:192.168.0.0``
The operation that leads to an ``192.168.0.0`` subnet is ``ip & subnet mask``

The subnet mask doesn't have to be composed of 0xff hex octates.
The subnet mask can mask any bits.

---