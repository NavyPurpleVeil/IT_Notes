Data link
---
An example data link is an ethernet frame, which recognizes the devices through the use of MAC addresses.

For example fiber cables typically either use ethernet frames or fiber channels(usually used by enterprise storage solutions).

Ethernet
---

Ethernet frames are terminated by 12 NULL bytes.
Which denotes the end of transmission.
Describes from who to whom to send the data.
As well as includes a simple CRC32 checksum.

| Layers | Preamble | Start frame delimiter | MAC destination | Mac Source | 802.1Q Vlan tag (optional) | EtherType(packet type) or length | Payload | CRC 32 bit | Interpacket gap |
| - | - | - | - | - | - | - | - | - | - |
|  | 7 octets | 1 octet | 6 octets | 6 octets | 4 octets | (42)46-1500 octets | 4 octets | 12 octets |

Preamble, start frame delimiter and interpacket gap are created in the layer 1(so likely by the hardware, ?the os just gives it the carc the checksum, payload and mac destination and mac source that matches the MTU size? ).

MTU size is the maximum of payload allowed.
Anything over the MTU size doesn't pass through, but it doesn't mean the size can't get divided up.

---

802.11
---

---

Fiber channel
---

---



---

