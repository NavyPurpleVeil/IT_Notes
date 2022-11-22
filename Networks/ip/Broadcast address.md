How is a broadcast address calculated?
---

subnet: ``192.168.0.0``
subnet mask: ``255.255.255.0``
broadcast: ``192.168.0.255``

Hence the operation is:
subnet + (subnet mask ^ 0xff 0xff 0xff 0xff)

---