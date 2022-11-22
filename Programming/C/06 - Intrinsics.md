What's an intrinsic?
--------------------
Intrinsics are a high-level abstraction allowing use of cpu extensions.
They are high level as they abstract the use of registers, as well as abstract away any inline assembly rules.
With an appropriate compiler flag, intrinsic abstraction for the most part are zero-cost.
Intrinsics were sometimes implemented as an compiler macro(always using MM0 and MM1 in case of MMX).

How to use intrinsics?
----------------------
```c
// Add header
#include <mmintrin.h>

// Adds 8 8Bit values together
__m64 add(__m64 vecA, __m64 vecB) {
	return _m_paddb(vecA, vecB); 
}

// 0-abstraction movq on x86
union evilMMX {
	char* str;
	__m64* mmx;
	long long* lon;
};

// Pseudo split inserts "/0", assuming non-repeating characters, this function "splits" the string.
// Assumes a string divisable and aligned to 8 bytes
char* split(char* string, char c) {
	char* ret = string;
	_mm_empty();

	__m64 stringMMX;
	union evilMMX evilMMX;
	evilMMX.mmx = &stringMMX;
	union evilMMX evilSTR;
	evilMMX.mmx = &stringMMX;
	evilSTR.str = string;
	(*evilMMX.lon) = (*evilSTR.lon);

	__m64 needle = _mm_set1_pi8(0xFF);
	__m64 mask = _m_pcmpeqb(needle, stringMMX);
	union evilMMX evilMASK;
	evilMASK.mmx = &mask;

	if ((*evilMASK.lon) == 0) {
		ret = NULL;
	}

	__m64 inverseMask = _mm_set1_pi8(0xff);
	mask = _mm_xor_si64(mask, inverseMask);
	stringMMX = _m_pand(stringMMX, mask);
	(*evilSTR.lon) = (*evilMMMX.lon);

	return ret;
}

```

Alternatives
------------

* Using a higher level language
* Creating an abstraction with C++ syntax overload
* Using openMP