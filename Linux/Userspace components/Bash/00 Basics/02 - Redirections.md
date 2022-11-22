# Redirecting output

Redirections, those are setup before the program starts:
```bash
< # replaces stdin(file descriptor 0) with a file
> # write and flush file/file descriptor
>> # append output to a file
<< # pipe into command/program
command_1<<EOF 
user input is treated as a pure string
$(command_2)
unless we use $(command_3), string gets interjected in the middle
EOF
command_1 | command_2 # pipe command_1 sends it's stdout into command_2, stderr isn't redirected to command_2

2>&1 # Redirect stderr(file descriptor 2) into file descriptor 1(stdout)
```

Stdout redirect back into bash shell:
``` bash
$() # This operator will inject space seperated arguments into an argv of a command, spaces can't be escaped by using ``\``.
```
If a command wrapped in ``$(command)``, contains ``&&`` operator and a like it's contents won't be treated like user input bash shell invocation.
Also incase of ``$(command)`` ``"`` isn't parsed and is ignored.