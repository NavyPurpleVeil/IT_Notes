
```bash
# foreach style for loop
for variable in "string "/$(command)/$variable/paths_wildcards_supported;
do
	command_1 $variable
done
```

```bash
# just like 
while(command); # can be $? and builtins: [[ ]] and [ ]
do
	command_1
done
```
```bash
# classic for loop
for ((n=0; n < ${#strarr[*]}; n++))
do
	echo "${strarr[n]}"
done

```