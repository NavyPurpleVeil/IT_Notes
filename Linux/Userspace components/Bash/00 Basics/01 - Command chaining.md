Flow control
---

Command chaining:
```bash
command_1 || command_2 # command_2 only executes on failure of command_1
command_1 && command_2 # command_2 only executes on success of command_1
command_1 | command_2 # pipe command_1 sends it's stdout into command_2, stderr isn't redirected to command_2
command_1 & command_2 # command_1 is running in the background and command_2 is run right away
command_1 ; command_2 # command_1 and command_2 are launched at roughly the same time

echo $? # prints the result of the last command
```

---