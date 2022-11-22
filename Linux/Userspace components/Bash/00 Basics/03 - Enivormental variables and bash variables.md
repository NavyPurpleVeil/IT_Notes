What is an enivormental variable?
---

It's an extra key-value store of arguments passed to a program as an array.

Bash shell keeps track of the list which it injects into programs, as well as it can use the itself.


#### Enivormental variables "persistance":

Enivormental variables can be set as follows during the bash session:
```bash
ENV_VAR=value command_1 && command_2 # This sets up an ENV_VAR for the command_1, command_2 is unimpacted by this
export ENV_VAR=value # We change the ENV_VAR locally to our shell session
command_1 && command_2 # The ENV_VAR has been impacted, and those both commands will see it's impact
source file.sh # We execute the bash script and source the enivormental variables set in the source file, if the ENV_VAR has been changed here it's changed, if not it was unimpacted;
./file2.sh # We execute a file(likely, bash script) which runs a new bash program, those any changes done to any Enivormental variables are kept within the scope of that bash session;
unset variable_name # removes the variable from the enivormental variables for the session
``` 

This can be visualized as follows:
```bash
$ PATH= ls
bash: ls: No such file or directory
$ PATH=/usr/bin ls
Android  Desktop    Downloads  Pictures    Public          Templates
Code     Documents  Music      Playground  StudioProjects  Videos
$ ls
Android  Desktop    Downloads  Pictures    Public          Templates
Code     Documents  Music      Playground  StudioProjects  Videos
$ export PATH=
$ ls
bash: ls: No such file or directory
$ export PATH=/usr/bin
$ ls
Android  Desktop    Downloads  Pictures    Public          Templates
Code     Documents  Music      Playground  StudioProjects  Videos
$ unset PATH
$ ls
bash: ls: No such file or directory
$ export PATH=/usr/bin
$ cat source.sh
#!/usr/bin/bash
export PATH=
$ ./source.sh
$ ls
Android  Desktop    Downloads  Pictures    Public          Templates
Code     Documents  Music      Playground  StudioProjects  Videos
$ source source.sh
$ ls
bash: ls: No such file or directory
```

Setting enivormental variables:
```bash
/etc/profile # Global to all users, now days just sources /etc/profile.d/*(.sh)
/etc/profile.d # folder containing enivormental variable setup bash scripts
~/.bashrc # bash settings, can also export env variables
~/.bash_profile # bash_profile is local to your user
# Bash run as ROOT only trusts /etc/profile, .bashrc and .bash_profile is ignored
~/.profile # Local to the user, set on login
```

Special libc enivormental variables:
```bash
LD_PRELOAD # Preloads a library before program execution
PATH # Impacts 3P exec*p, which visually impacts the bash shells search path, when set for a user .profile it impacts other programs PATH;
LD_LIBRARY_PATH # Sets the path to search dynamic libraries in; "/lib64/ld-linux-x86-64.so.2" < search is conducted by a special dynamic loading library, ld.so.conf/ld.so.conf.d
LC_* # modifies the language parameters
```

