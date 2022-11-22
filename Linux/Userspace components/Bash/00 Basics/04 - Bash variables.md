Bash variables
---

Bash variables always have an string type;
Integer arithemtics are possible with a let built-in.

---

All enivormental variables are Bash variables, but not all Bash variables are enivormental variables:
```bash
variable=value
export variable # links the bash variable into an enivormental variable, any changes of the variable will now carry over into the enivormental variable
env | grep variable
#output: variable=value
variable=value2
env | grep variable
#output: variable=value2
```

Special bash variables:
IFS:
```bash
IFS= # Sets an string separator character for iterating through the strings; 
https://bash.cyberciti.biz/guide/$IFS#IFS_Effect_On_The_Values_of_%22$@%22_And_%22$*%22
IFS="|"


#!/bin/bash
# Doesn't impact $@;
# Impacts $*;
IFS="|"
echo "$@" # Output: arg1 arg2 arg3
echo "$*" # Output: arg1|arg2|arg3


IFS=":"
for x in $PATH; do echo $x; done
/usr/local/sbin
/usr/local/bin
/usr/bin
/var/lib/flatpak/exports/bin
/opt/flutter/bin
/usr/lib/jvm/default/bin
/usr/bin/site_perl
/usr/bin/vendor_perl
/usr/bin/core_perl
```
PS1-PS4:
```bash
Usually set in bash_profile/bashrc, in bashrc/bash_profile accepts extra parameters
PS1= # Command prompt
PS2= # Prompt to complete a command on the next line 
PS3= # Question chars on select prompt(creates a list out of a space seperated string)
PS4= # Debugging mode variable bash -x
```

Bash variables can be injected into a command as argument/run as a command or used to modify the ENV_VARs or bash variables:
```bash
export PATH=$PATH:/some/different/path # appends the path
export name="ffmpeg -i && echo dummy" # export a bash variable as an enivormental variable?
$name
# Let's look at it and reiterate:
# this will execute "ffmpeg -i and && echo dummy" a bash command
# As opposed to:
$(echo ffmpeg -i \&\& echo dummy)
# this will execute ffmpeg, with arguments: -i && echo dummy

name="&& echo dummy"
$(echo echo sure $name) # the variable inserted into $() is interpreted as a string
sure && echo dummy
```

---