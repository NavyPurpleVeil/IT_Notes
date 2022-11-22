 IP protocol
---

Devices in an ip protocol are identified by their ip address.
Their mac address(and vice versa) is found out using the ARP protocol.

---

IPv4
---
#### Address representation

---

#### IPv4 frames can be fragmented.

20-60 bytes is used up by the ipv4 header
1500-60 = 1440

The MTU size is a thing that defines an individual link's negociated frame size.

Frames that contain a higher MTU can't be accepted, and those need to be divided, by the requester.

As frames pass through the router, if the frame is too big for the upper router to handle, the router has to divide up the IP packets and send it as (at least) 2 ethernet frames to the next router.
If the MTU of the device is below 576+(60/20?I mean it's ipv4 spec, so for the eth frame it must include ipv4 header size?) Bytes, it isn't up to spec.

It's advised to avoid IP protocol dividing your ethernet frames by 2, to do so MTU path discovery must be implemented.
There's some way to mark the packet as non fragmentable, a response must be invoked by the router, but it often gets "blackholed"(filtered out or otherwise).
[defacto unimplementable in practice, through it's intended way]

The only way to implement MTU discovery is by sending IP datagrams and counting the number of ethernet frames received that contained the IP packet.
[the IP packet obviously needs to be numbered for the data to be recoverable in order].

---
### NAT

NAT often acts as a semi-firewall.

##### Types of NAT:
>* Full-clone NAT
> An internal address+specific port(s) are mapped to an external address+specific port(s)
> Any host can send packets to this mapped address

>* Address restricted-cone NAT
> As above.
> The mapping is restricted, the communication needs to be initialized by a specific ip address.

>* Port-restricted cone NAT
> As above
> The mapping is additionally restricted to the port of the specific host(say port 443, can't send data back to the port which was openned when the client communicated via port 80).

>* Symmetric NAT
> One client, many external ip addresses, each outcoming connection requires an additional external ip address for the Symmetric NAT to function.
> Server can respond only to the specific ip address.


---

IPv6
---

#### All ipv6 addresses are public, NAT doesn't exist those proper on-device firewall configuration is required.

#### IPv6 frames can't be fragmented.

ipv6 specification doesn't allow any router to "divide" the frames into X(2) frames.
Any frame that the router can't handle ?likelly will get their frames dropped?.

The minimum MTU an IPv6 compliant router must support is 1280 Bytes.

---
