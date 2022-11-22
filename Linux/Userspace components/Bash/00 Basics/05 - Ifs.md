IFs
---

Bash doesn't denote code blocks with ``{ }`` characters, and supports standard features if..elif..else, for and while loops:
```bash
if # no condition 
then
	command_1
elif
then
	command_2
else
then
	command_3
fi
```
 
Bash conditionals are a bit different from regular programming languages:
```bash
if [[]] # Variables are literal strings; Allowed operations == !=
then
	command_1
fi

if [] # Variables aren't literal strings, a command with arguments can't be put into here
then
	command_2
fi

if (command_3) # Grabs the $? of the command running within, variables aren't string literals
then
	command_4
fi
if (($a > $b)) # Compares variables, values are integer literals
then
	command_5
fi
if []
```

How do those typical conditionals work in bash ``()`` ``(())`` ``[[]]`` and ``[]`` 
```bash
$a=10
$b=bb
if (( $a <= $b )); then
	echo "$a" #prints nothing
	#statements
fi
$b=12
if (( $a <= $b )); then # != >=  <= ==  >  <
	echo "$a" #prints 10; 
fi


if [ "$(ls)" < "$(ls /)" ] ; then echo 10; fi
10
if [ "$(ls)" > "$(ls /)" ] ; then echo 10; fi
10
if [ "$(ls)" == "$(ls /)" ] ; then echo 10; fi
if [ "$(ls)" != "$(ls /)" ] ; then echo 10; fi
10
```


---