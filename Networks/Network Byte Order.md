What's a Network Byte Order
---
Computer systems typically come with 2 different Byte order representations.
little endian and big endian.
Majority of computers are little endian.
Networks Byte Order is big endian, most protocols use the big endian.

Imagine a medium size number(bigger than 255 is enough).

256, it will be represented in binary as follows:
0b100000000 

The lowest value that is usually retrivable and stored in memory is 8bits, so our number looks like this:
0b00000001 0b00000000 (big endian)

Another processor might represent such value as:
0b00000000 0b00000001 (little endian)

And so this representation might be even further apart(for a 32bit number):
0b00000000 0b00000000 0b00000001 0b00000000 (big endian)
0b00000000 0b00000001 0b00000000 0b00000000 (little endian)

---