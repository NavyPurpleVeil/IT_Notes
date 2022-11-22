What is a checksum?
-------------------
Checksum is a kind of transformation of the input data.
By re-doing the checksum we can find out if the data has been damaged.
However we cannot verify if the data has been tampered without using, public+private-key pairs.
Cryptograhically signed documents, have checksums which can be verified.


What are the uses for the checksum?
-----------------------------------
Some checksums are used for ErrorCorreCtion like CRC32.
General purpose checksums can't be used for ECC, only for ErrorDetection(transmission failures etc.).
Some checksums don't need to be cryptographically strong, they just need to be fast - xxhsum checksums see wide use in non-mission critical use cases.

Passwords are transmitted directly over TLS to the server.
Reasoning?
If a checksum would get transmitted, then a leaked password database could be used as if no hashes got used.


What are hash colisions?
------------------------
Due to nature of a hashed checksums, being a heavily shortenned, non-recoverable data.
Hash colision happens when there's another piece of data which when computed causes the same kind of hash to be generated.
Hash colissions can cause much longer passwords to be re-solved by a much shorter one.
At the very worst hash colissions are predicctable and data can be freely altered.


What is salt?
-------------
Salt prevents rainbow tables from being effective.
Rainbow tables are precomputed hashes.
For brute forcing on a leaked database, salt increases complexity of bruteforcing from O(N) [where N = combinations (...) length-1^Combinations * length^Combinations] to O(N^M), where M is number of unique salts.


How to compute a hash?
----------------------
```bash
$ sha512sum file
9a8be07a7916cbca8a234b71777fe245567dcf7dbfe768a8fe84064af8d07377f1342298578b35b61ce5adc517c01610fdb2e1466bbf6d072be0dd849687b3ea  file
$ dd if=/dev/sda bs=8M ioflag=fullblock | sha512sum -
9a8be07a7916cbca8a234b71777fe245567dcf7dbfe768a8fe84064af8d07377f1342298578b35b61ce5adc517c01610fdb2e1466bbf6d072be0dd849687b3ea  -
```