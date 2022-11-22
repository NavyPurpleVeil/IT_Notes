Bit operations
---

All binary operations in C are done in Big Endian;
// architecture dependent maybe?

```c
~ // not
& // and
^ // xor
>> // shift right
<< // shift left
| // or
```

The properties of the various binary operations can serve a couple of simple purposes:
```c
|= // set
& // apply mask
~ // flip bits (0xFF\*bytes)
^ // flip bits with a mask
>> // divide by powers of 2, set in position 
<< // multiply by powers of 2, set in position
```

It can be used to avoid branching on cpus in which conventional operations are less costly than a branch miss.
Avoiding branching at all cost isn't always going to improve performance.


Structs can align variables of variable bit-length which otherwise wouldn't be aligned;
```c
struct rgba5551 {
	uint16_t r:5;
	uint16_t g:5;
	uint16_t b:5;
	uint16_t a:1;
}; // This struct's size is 16bits 2 bytes, contains 3 5 bit variables and 1 1 bit variable;

// This struct can be accessed as a variable with either a union or cast
```


---