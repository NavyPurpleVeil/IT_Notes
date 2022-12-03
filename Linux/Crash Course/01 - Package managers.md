What is a package manager?
--------------------------
Package manager is simply "the installer" program.
Longer form it's nothing more than a program solving issues related to program installation and management.
Since ubuntu(2008) days the GUI for underlying package managers started to take a form of an app store.
So you might never have to touch the commandline(until you deal with something that's hidden from the store).

What exact problems do package manager solve?
------------------------------------------------
* (nix, hurd)Replicable configuration of the system, programs and it's components.
* Being able to remove every no longer needed dependency that a program dependent on.
* Having a one way stop to download and remove any program you want.
* Verifying that a program wasn't tampered with(this usually occurs when pulling packages from the repository; anything 3rd party either requires a repository with it's own set of keys; if it's a downloadable package it likely has different forms of manual verification).
* (zypper, dnf, apt, nix) Versioning issues (2 programs might want to use the exact same library, but they might have been compiled for a different version of it)

Why are dependencies shared across the entire system?
-----------------------------------------------------
Primary benefit is lower ram usage, and with it better cpu cache utilization as programs share the libraries, they too share the physical location of code in ram.

There's a lesser known issue(I had it happen on me), just like you might enter the linux dependency hell(which is when your program doesn't start, because of a missing or incorrectly fullfilled depepdency :O, someone made an oopsie and it either was you or the guys behind the repository updates).
You may enter DLL hell, which is just a fancy way of saying "program on windows doesn't work the way it should, and I just had to copy and paste this file into it's directory".
VLC might have had an outdated subtitles library which causes it to crash on say ~~No longer applicable~~ crossed out words.
And this issue wasn't replicable on linux(ubuntu 22.04), nor with another video player software on windows, and the issue went away by copying the dll from the other video player.

Linux dependency hell happens, because of this very thing basically we drift around the entire problem of having an old library by updating the version of libraries for the entire system (warranted that they don't change the api, the repository manager just got scott free).



