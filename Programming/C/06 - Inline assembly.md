Inline assembly
===============

```c
	// The operands are reversed in the AT&T syntax as oppossed to Intel syntax;
	// AT&T syntax is used in inline asm
	// Intel syntax(definietly for x86);
	__asm__("vmovntdq %%ymm9, (%0)":: "r"(&output[160]));
	// inline assembly in C, C symbols can be  ^
	// Last arg of __asm__ divided through ":", 

	// There are intrincs(headers which devise a certain value) for various cpu extensions
```

```c
asm() {

}
```

Link to the website which describes more:
```url

```