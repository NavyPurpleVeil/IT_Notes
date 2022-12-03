Everything is a file?
---------------------
Yes, but actually no.
What this sentence means is that various interfaces for devices or apis might happen over standard IO(so regular file write/read operations).
And most importantly that they are a vissible file somewhere on your computer.
(They clearly are not always vissible).

They don't always happen over standard IO
----------
Well the truth be told not everything just happens over standard write/read operations.
Which is where ioctl() io controls syscall comes in, it let's you modify the behavior of a device/driver (a program needs to use ioctl to unlock the bluray drive when you have a protected video disc).
The interface I heard is despised is the USB ioctl() interface, where presumbly everything happens over ioctl()s.
(Never used the api, I just know libusb exists that is a cross platform way to deal with usb.)

They aren't always vissible
-----------
Well your shared memory object.
(we will disregard existance of a file for shared memory the /dev/shm as we can both use sysV api or POSIX api).
Or your message queue, doesn't actually exist on a path in the filesystem, it has it's own internal path that you can't access.


What is a syscall?
------------------
It's a function programs can call, depending on whether the program can do an action, the system will return a success or some kind of failure(usually the failure is further explained via ERRNO and context the programmer has, and likely should share to the user).

