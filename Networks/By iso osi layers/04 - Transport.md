Transport protocols
---

#### Port forwarding
It's the practice of moving UDP/TCP packets to a specific port+interface pair from a specific port+interface pair.

---

UDP
---
UDP is a simple connection less datagram protocol, featuring no guarantees about the data being received by the other party, it's order.
The UDP server knows the location of the client(it's address and port, which we can just send back data into).

It just features(in order) the source port(16 bit), destination port(16 bit), data length(16 bit), optional checksum(16 bit)(IPv4).

Often times for compatibility with current routers, other transport layers protocols are implemented on top of UDP, this provides a custom behavior and feature set without having to use full blown TCP and all it's features and the ways they have been implemented. 
Example of such protocol is RPUDP used by nintendo 3ds.

---



TCP
---

Unlike UDP, TCP requires establishing a connection.


#### TCP is a "streaming socket"

The programming interface available to applications usually abstracts away the needs to send specific types of TCP data packets(datagrams).
And establishing the connection is simplified into a ``connect()``  syscall.

As a programming interface TCP is accessed as a stream of bytes, but it is commonly confined within ethernet frames, and the data is send as datagrams.

It is possible to lookahead of what you would normally read.
This causes the initial offset bytes to be skipped.
The lookahead portion read into the buffer, but also discarded on future read, which starts without the offset.
 
Once the connection has timed out or has been closed, we aren't allowed to send data back(also such attempts would end up in failure, as the operating system on the receiving end will drop out packets).

#### TCP packets

| Offset | 0 | 1 | 2 | 3 |
| - | - | - | - | - |
| 0 | Source port | <- | Destination port | <- |
| 4 |  Sequence number | <- | <- | <- |
| 8 | Acknowledgement number | <- | <- | <- | <- |
| 12 | Data offset(4 bits) reserved(3 bits) NS(1 bit) | (1bits) CWR, ECE, URG, PSH, RST, SYN FIN | Window Size | <- |
| 16 | Checksum | <- | Urgen pointer (if URG set) | <- |
| 20-60 |  Options if data offset > 5. PAdded at the end with "0" bits if necessary.) | <- | <- | <- |




#### Tcp thingies

>**TCP heartbeat.**
Is otherwise known as sending na empty packet to avoid the timeout of a connection.

>**TCP 3-way handshake.**
Is the initialization of a tcp connection:
>1. Client sends a SYN tcp packet, with a certain sequence number(n);
>2. Server sends back a SYN packet(m) + ACK packet(n+1).
>3. Client sends back a ACK packet(m+1).
>```bash
tcpdump -i lo
11:24:06.238794 IP localhost.52988 > localhost.http-alt: Flags [S], seq 2255179786, win 65495, options [mss 65495,sackOK,TS val 2085134965 ecr 0,nop,wscale 7], length 0  
11:24:06.238802 IP localhost.http-alt > localhost.52988: Flags [S.], seq 2404390255, ack 2255179787, win 65483, options [mss 65495,sackOK,TS val 2085134965 ecr 2085134965,nop,wscale 7], length 0  
11:24:06.238807 IP localhost.52988 > localhost.http-alt: Flags [.], ack 1, win 512, options [nop,nop,TS val 2085134965 ecr 2085134965], length 0
>```

>**TCP window size:**
>* Congestion window Limits the manimum amount of data that can be "out in the wild" and unacknowledged, before sending another packet.
>* Receive window is the Limit requested by the client.

>**TCP timeout.**
>onnections automatically get disconnected on a timeout.

>**TCP Congestion algorithms, their role.**
>
There's around 20 tcp congestion algorithms in the linux kernel.
>
The role of an Congestion algorithm is to:
>1. Avoid oversaturating your connection.
>2. Avoid flodding the router(reduce the number of dropped packets).




---