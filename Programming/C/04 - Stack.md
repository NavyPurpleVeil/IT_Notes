Stack
---

Values that are allocated ahead of time are placed on a stack which grows upwards[When imagined 0x00 as top and 0xFF as bottom].

Stack other than local function variables, holds cpu register state.
Those get saved on a stack when a new function gets invoked.

Register status is restored by the operating system and stored on a stack (during syscalls, the app puts register status on stack and restores it once the call ends) (when task gets preempted, the os takes care of it).

Declared variables when read in source code, will appear in their original order on the stack. 
When variable re-ordering is enabled this will not be the case.


#### Stack modification
```c
// I know it's possible to implement cooroutines in C
// There are public symbols exporting the return values used by longjmps
```


---