Pointer arithmetics
-------------------

Pointer arithmetics is applicable to pointers only.
Pointer arithmetics, is just Regular arithemtics multiplied by the size of the base type the pointer points to.

```c

sizeof(char) // returns 1
sizeof(int) // returns 4

int* intPtr = NULL;
intPtr += 5; // 0+5*4; 20
intPtr *= 2; // invalid operator
intPtr -= 5; // 20-5*4
```

-------------------