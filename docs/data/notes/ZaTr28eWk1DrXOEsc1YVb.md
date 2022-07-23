
All user software needs to go through the operating system in order to use any of the hardware, whether it be as simple as a mouse or keyboard or as complex as an Internet component.

### Parts of the OS
The [[kernel|os.kernel]] is the brain of the OS, but other parts provided by the OS are:
- User interface
- security
- networking ability
  

### User space (userland) and Kernel space
To mediate access to the same resources, the [[kernel|os.kernel]] has special rights, reflected in the distinction of kernel space from user space.

#### Kernel space
Kernel space is strictly reserved for running a privileged operating system kernel, kernel extensions, and most device drivers. Anything running with this privilege is fully trusted and has full control over the OS.

Kernel space can be accessed by user module only through the use of system calls.

#### User space
User space is the memory area where application software and some drivers execute. Processes running here are subject to the Principle of Least Privilege.
- *userland* refers to all code that runs outside the operating system's kernel. These are programs and libraries that the operating system uses to interact with the kernel.
- ex. bash, vim, VSCode, WoW etc.

