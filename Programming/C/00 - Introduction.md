Introduction
---

### Keywords
Keywords let's you modify the behavior of the language in a predefined manner.

#### Const arguments
Using keyword const on an argument of a function will mark the variable as read only.
```c
int copy(const int* src, int* dst) {
	*src = *dst;
	// Thanks to the const keyword, value will not get assigned to the src argument.
}
int copy(const int* src, int* dst) {
	*((int*)src) = *dst;
	// Value has been assigned, because the cast no longer contains the const keyword.
}
int copy(int* src, int* dst) {
	*src = *dst;
	// 
}
```

#### Static variables within function scope
Static keyword when combined with a variable that is part of a function rather than lose it's value(default behavior), it will carry over it's previous value across subsequent function calls.
```c
int counter() {
	static int count = 0; // initialized once
	count++;
	return count;
}
int main() {
	counter(); // returns 1
	printf("%d\n", counter()); // prints "2"
	// Conclussion: value carries over across counter() function calls.
	// Function can be called by any other function, the value will carry over. 
}
```

#### Static functions
Static functions can't be included by another file, so they can't be used outside of the file which defined them.

#### Extern
Function is exported for other files to use.

#### Register
Suggests to the compiler to store the value in cpu registers.
Causes the value in an argument to be stored in a register.

```c
void copy(int* src, register int* dst) {
	dst = src;
	src = &dst; // dst in register, no pointer to this value can be made.
}
int add3(int a) {
	// This isn't a good use of it
	register int b = a+3;
	return b;
}
```

#### Inline keyword
Inline keyword is a suggestion to the compiler, that a function should be inlined.
Difference between inlined function and un-inlined function is the same as pointing to a page in a textbook and writting down the relevant information from the textbook in your notebook.

Un-inlined functions can be more cache friendly(less duplicated assembly code == less cache juggling).
Inlined functions can be faster(one less cpu register state save+restore).

```c
inline int add(int a, int b) {
	return a+b;
}
int main() {
	int c = add(2,6);
}
```

#### Volatile variable
Informs the compiler that a value shouldn't be optimized out.
As in it's it marks the varriable as an address location that the system/hardware will modify.


### Functions

Function prototype looks as follows:
```c
returnType symbolName(argumentType1 argumentSymbol1, argumentType2 argumentSymbol2, ...);
// ... == variable argument count
// Varriable Argument Length functions need to handle the extra arguments provided
// 
// There can exist multiple definitions of one symbol function  
```
Function prototypes differ from function definitions in that they aren't implementing the functionality of a function.
They just give a blueprint for the number and type of arguments(and those registers involved).

#### Arguments are always copy by value.

Arguments are always passed by copy, what this means is to change the object(or pass an object by reference), you need to provide a pointer to the object you want to change.
```c
void copy(int src, int dst) {
	dst = src; // won't work
}
int src = 5;
int dst = 0;
copy(src, dst);
// dst == 0

void copy(int src, int* dst) {
	*dst = src;	// Insert value of src to memory address location dst; 
	// *dst == Pointer dereference;
}
int src = 5;
int dst = 0;
copy(src, &dst); // &dst == Passed a pointer to dst;
```


### Header files

Header files provide function prototypes, function definitions(shouldn't unless it's meant to be an inline function), and macros.
Header files can be used internally, or provided for others(headers make apis possible).

#### Header guard
Header guard prevents including the same header twice. 

```c
#ifndef __HEADER_NAME_H_
#define __HEADER_NAME_H_ 1

void copy(int* dst, int src);

#endif
```


### Variables, symbols, scope


#### Variable definition
```c
typedef int symbolType;
symbolType symbolName[arraySize];
```
Using uninitialized variables is undefined behavior, always initialize the variables.

```c
int a = 0; // initialized to zero
int b; // uninitialized can contain garbage data
// Why unitialized variables can contain garbage data:
// 1. Stack memory was used by another function.
// 2. System depending on it's configuration can: a) initialize memory to zero on reclaim. b) initialize memory to zero on assignment. c) leave the memory dirty.
```

#### Symbols

Not every symbol is a variable, but every variable has a symbol.
Not every symbol is a function, but every function has a symbol.
Not every symbol is a macro, but every macro has a symbol.

Symbols are simply put identifier names for variables, functions and macros.
Symbols are unique within their own scope.
By design each symbol is unique from each other.
Only macros can be undefined. 

#### Scope

Scope defines when things are and aren't accessible in the code context.

Keeping track of scope and heap allocated memory is important.
When a reference to an allocated memory location gets lost it's resources don't got freed.
When a symbol isn't in your current scope it simply can't be accessed.


```c
int name; // global varriable, global varriables are accesible anytime anywhere.

int add(int a, int b) {
	int c = a+b;
	return a+b;
}

// Unless provided by a header a function isn't in a global scope.
int main (int argc, char** argv) {
	#define macro(a)\
	 a*2 // This macro is accessible from this line onwards.
	int a = macro(2); // a = 4
	#undef macro // macro is now unaccessible
	a = add(a, 2); // a = 6 
	{
		int c = add(2,a);
	}
	for(int i=0; i!=1; i++) {
		// i is accessible
	}
	// c and i is inaccessible
	malloc(10); // Memory leaked;
}

```


#### Memory alignment:
Variables, need to be memory aligned to their respective data size in order to be properly read, when instructed(-Optimize flags) C compiler can re-order variables, in order to reduce memory use.
```c
	char a; // 8 bit
	short b; // 16 bit
	int c; // 32 bit
	long d; // 64 bit
```

The original order will actually be:
```c
	char a;
	// <8bit padding>;
	short b;
	// <0/16 bit padding>;
	int c;
	// <0/32 bit padding>;
	long d;
```
This uses up to 56 extra bits. When using an array of structs the size used increases linearly.


A memory aligned declaration/most likely re-order will look as follows:
```c
	long d;
	int c;
	short b;
	char a;
```
This way 0 bits are lost to alignment, if no other variables are present, then doing a 15 byte memcpy from &d should copy over all variables in their respective order, variables put into a struct are more likely to be grouped together.

Memory alignment is especially important when dealing with low-level functionality for example to use AVX, SSE, MMX registers, the memory which we place items into and from needs to be Register size aligned. 

And when we deal with external data which we want to import straight into our data structures.

#### Memory packing:
Packing, on the other hand prevents compiler from adding padding.
It will re-arrange the structure to avoid padding.
```c
struct __attribute__((__packed__)) structure {
    char a;
    int b;
    char c;
};
```

### Enums
Enumerators provide unique sequence of bytes.
```c
enum httpCodes {
	HTTP_OK = 0,
	HTTP_NOT_FOUND,
	/* next entry increments by 1 */
};
```
Each ENUM entry becomes part of the scope they exist in.
Enums can be "instructed" as to how they decrement or increment.
```c
enum httpCodes {
	HTTP_OK = 0,
	HTTP_NOT_FOUND = -2,
	/* The next entry will decrement by -2 */
};
```

### Basic types

```c
char; // (Usually) and 8bit type
short; // 16bit
int; // 32 bit
long; // 64 bit
long long; // 128 bit

float; // Single precission float;
double; // Double precission float;
long double; // Quad precission float;
```

### Pointers
Pointers point to memory locations.
Pointers can be pointers to other pointers.

```c
	int a = 0;
	int* aPtr = &a; // Set pointer value to memory location of a
```
Dereferencing is act of modifying pointer access;
```c
	aPtr = &a; // Set pointer to memory location
	*aPtr = 0; // Set @pointer location;
	aPtr[0] = 1; // Set @pointer location, offset 0;
	// Offset follows pointer arthimetic logic.
```
Pointers arithmethic is different than that of regular arhitmethic.
```c
	// 
	int a;
	int b;
	int* aPtr = &a;
	aPtr++; // == aPtr + X*sizeof(int)
```

#### Pointers to memory location containing functions
```c
	void copy(int* dst, int src);
	(void)(*symbolName)(int*, int) = &copy;
```

#### Array to pointers:
```c
char name[100];
// Size information gets lost when converting to pointer
char* namePtr = &name;

void func(char name[]); // Any size array, size gets lost.
void func(char* name); // Accepts both an array and a pointer.


```

#### Pointers as arrays:
```c
	int*** arrayOfAraysOfArrayOfInts = malloc(sizeof(int***)*10);
	for(int i=0;i!=10;i++) {
		// don't assume memory got allocated
		arrayOfAraysOfArrayOfInts[i] = malloc(sizeof(int**)*10);
		for(int n=0; n!=10; n++) {
			arrayOfAraysOfArraysOfInts[i][n] = malloc(sizeof(int*)*10);
		}
	}
```

### Arrays

#### Variable Length Array
```c
	int n = getLength();
	char name[n]; // Length depends on the return of the function.
	#define len 100
	char name[len]; // Length predefined static length array.

```

### Casting
Casting types modifies the type that is modified.        
Casting can be done on Lvalue and Rvalue.        
Lvalue casting is temporary.       

Casting pointers+arrays is influenced by byte order.       
Casting basic types isn't influenced by byte order.      

```c
alignas(sizeof(int)) char name[4] = {0x00, 0x01, 0x02, 0x03};
printf("%d\n", ((int[1])name)[0]);
```

### Branching

```c
if(conditional1) {

} else if(conditional2) {
	// only triggers when conditional2 returns true and conditional1 returned false
} else {
	// anything goes
}

if(conditional1) {

}
if(conditional2) {
	// conditional 2 will be verified even if 1 hits
}

(conditional) ? onTrue() : onFalse() ;
```

### Loops

#### For loops
```c
for(executeBeforeLoop ; conditional; executeAfterOneIterationOfTheLoopCompletes ) {

}
```
#### While loops
```c
while(conditional) {
	// Executes until conditional isn't met
	// Conditional is checked first
}
```

#### Do-While loops
```c
do {
	// Always executes at least once, and then the conditional is checked
} while(conditional)
```

#### Misc
Loops can be influenced by ``` continue; ``` and ``` break; ``` operations.

```continue;``` - continues the loop;
```break;``` - ends the loop regardless of whether the condition of the loop is still met;

Structs can be anonymously initialized in for loops like so:
```c
for(struct { int a; int b; } s = {0, 2}; a != b; a++ ) {

}
```

### Structs
They hold multiple basic and advanced types.
With structs you can implement interfaces, and object-like structures.

Structs can be initialized on assignment.
```c

struct sockadd_in addr = {
	.sin_family = AF_INET,
	.sin_port = 1025,
	.sin_addr = inet_addr("192.168.0.1")
};

struct pollfd polfd = { 
	argc,
	POLLIN|POLLHUP,
	0
};

```

Structs can be anonymous(typeless)
```c
// for loop anonymous struct declaration
for(struct {int a; int b;} s = {1,2} ;; ) {

}
```

### Unions 
Are alternative representations of data types.
You can think of it as a data type that has a predefined cast.

```c
struct rgba16 {
	short r:5,
		g:5,
		b:5,
		a:1;
};
union rgba16Alt {
	struct rgba16 individual;
	short data;
};

union rgba16Alt exampleData;
exampleData.data = grabPixelData(0,0);
// We can now access the individual color channel data:
exampleData.individual.r; 
exampleData.individual.b; 
exampleData.individual.g; 
exampleData.individual.a; 

// We can modify the data as the other data type
exampleData.individual.r = 15;
storePixelData(0,0,exampleData.data);
```

### Switch case
```c
switch(variable) {
	case <value1>:
		action1; // value1 executes action1; action2;
		// fallthrough
	case <value2>:
		action2; // value2 executes action2;
		break;
	case <value3>:
		action3;
		break;
	default:
		action4;
		break;
}
```
In version C2X fallthrough might need to be verbously indicated.

### Macros
Macros are a pre-processor output.
As such they don't hold runtime information. 

#### Regular macros
Variables can be modified with a macro.
```c
#define macro(x,y,z) \
	z = x+y
```

#### String macros
Strings can be concated at the pre-processor level
```c
#define FUNCTION_NAME(name) #name
#define TEST_FUNC_NAME  FUNCTION_NAME(test_func)

#include <stdio.h>

int main(void)
{
    puts(TEST_FUNC_NAME);
    return(0);
}
```

### Generics
C generics are "special" macros which also take in the type of the varriable and apply code dependent on varriable type.     
The provided variable can be casted(for example you can include the type into a struct via ENUM and cast depending upon it, having both a generic catch-all interface structure, and actual type specific behavior).    

```c
#define cbrt(X) _Generic((X), \
	long double: cbrtl, \
	default: cbrt, \
	float: cbrtf
)(X)
```

### Interfaces
Pointers to functions alongside structs can be used to have implementable interfaces.
Example of this can be seen all over the Linux kernel.


### Linking
Linking is a process that allows multiple compiled source files to be connected together into a binary/library.

Dynamic linking allows to dynamically link libraries, this also allows updates to the library, when the API didn't change the ABI.

Static linking includes the library in the executable or library.

---