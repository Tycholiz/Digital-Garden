
The kernel is the part of the OS that serves as the master control program of all processes thought of as back-end.
- it is the lowest level of [[general.terms.abstraction]] in the [[os]].
- It provides an interface to hardware devices, and manages system processes, memory and other resources

The OS runs the kernel as fully trusted software. Once root access (kernel space) is granted to the kernel, a user can command the OS to do anything.
- therefore, the kernel is what must implement protection mechanisms that cannot be subverted by untrusted software executing in [[user space|os#user-space-userland-and-kernel-space,1]].

The [[operating system|os]] uses the kernel to perform grunt work involved in bridging the gap between the computer's hardware and its software.
- the kernel talks to the hardware via drivers (which are loaded as kernel extensions) and/or firmware
- ex. when an application wants to change the system volume, it submits a request to the kernel to perform the action. Because of the speaker drivers which it has access to via the kernel module (like a built-in extension; provides the same benefit), the kernel can control the volume

The kernel provides services that...
- handle I/O
- maniupulate the file system
- start and stop programs
- schedule access to avoid conflicts when programs try to access the same resource or device simultaneously. 
- handle other common "low-level" tasks that most programs share, 

The kernel also plays a big part in resource management, managing resources like memory and CPU.
- the kernel must manage how memory is used, which means it has to be fully aware of how it is being used. It has to know exactly where in memory any process's resources will reside.
- naturally, the kernel tries to optimize the usage of the resources it manages. It is fine if memory usage is high, even for moderate usage. The kernel knows how to remove things from memory in order to make space for the higher priority data.

Failures can lead to deadlocks, resulting in the whole system halting because resources used by one application are needed by another.

Technically speaking, [[Linux|linux]] is not an operating system; it refers to the kernel itself. This misunderstanding is somewhat justified, given how integral the kernel is to the [[os]]
- examples of operating systems in Linux are Ubuntu, Mint, Debian etc.

Like any piece of software, the kernel is versioned. If we want to see our kernel and its version, issue:
```sh
uname
uname -r
```
