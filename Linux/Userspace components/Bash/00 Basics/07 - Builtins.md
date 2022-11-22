Builtins
---

* read
* let  # conduct Integer type operations
* time
* ``[ ]``
* ``[[ ]]``
* ``( )``
* ``(( ))``

---
Let
---
let buitin transforms the string if possible to an integer;

Without let bash variables are operated on as if they were string type;
If the variable can't be transformed to an integer it will assume the value of the variable to be 0;

```bash
a=2233
a+=1 # string arithmetics, only += and = is allowed
echo "$a" # 22331
let "a+=1"
echo "$a" # 22332
b=$(a/22/BB) # BB331
let "b+=1"
echo "$a" # 1
```

let (?can't do floating point arithmethics?), always rounds down floating point numbers when possible
When number is rounded down to zero, let returns EXIT_FAILURE

---
Time
---

Prints the time of execution:
```bash
time mpv '/home/jakubby/Music/Electronic/The Prodigy - Out Of Space.flac' 
 (+) Video --vid=1 [P] 'Album cover' (png 600x519 1.000fps)
 (+) Audio --aid=1 (flac 2ch 44100Hz)
File tags:
 Artist: The Prodigy
 Album: Out Of Space
 Date: 1992
 Genre: Drum & Bass
 Title: Out Of Space (Techno Underworld Remix)
 Track: 02
Displaying cover art. Use --no-audio-display to prevent this.
VO: [gpu] 600x519 rgba
AO: [pulse] 44100Hz stereo 2ch s16
AV: 00:03:41 / 00:03:42 (100%)

Exiting... (End of file)

real    3m42,664s # Real human time of execution
user    0m1,016s # Cpu time spend in userspace
sys     0m0,484s # Cpu time spend in kernel space
# (user/real/cores*100) + (sys/real/cores*100) == average cpu usage per core spent on task)
```

There's also a more detailed version using ``/usr/bin/time``, which supports extra arguments;

---

``[ ]``,  ``[[ ]]``, ``( )``, ``(( ))``
---


---
Read
---

```bash
text="oh wow"
IFS=" "
read -a strArr <<< "$text"
for word in "${strarr[@]}"; do
    echo $word
;done
```
---

---