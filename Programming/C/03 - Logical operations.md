Logical ops
------------

```c
&& // logical and
|| // logical or 
!  // logical not
== // equals
!= // doesn't equal
>  // is more than
<  // is less than
>= // is more or equal than
<= // is more or equal than
```

Logical operators follow an order by which the logic operators get resolved
When unsure brackets can be used.
```c
// Both of these resolve to the same statement:
if(a >= b || a <= b) {

}
if ((a>=b) || (a<=b)) {

}
```

Logical operators can be used with regular arithmetics, they will resolve down to true or false:
```c
3*(true == 1)+3*(false == 0);
// 3*1+3*1 = 6;
```


------------