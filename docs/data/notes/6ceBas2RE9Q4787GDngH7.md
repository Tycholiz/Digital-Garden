
# Virtual Maachines (VM)
A VM is made up of a Host VM and a Guest VM

### Host VM
- the server component of a VM, which provides resources to a Guest VM, such as processing power, memory, disk, network I/O etc.

### Guest VM
- can exist on a single phsyical machine, but is usually distributed across multiple hosts for load balancing.
- The guest VM is not even aware that it is a guest VM, and therefore is not aware of any other physical resources that are allocated to other guest VMs.

### Hypervisor
- a piece of software in a computer that will create and run virtual machines
- the hypervisor intermediates between the host and guest VM, which isolates individual guest VMs. This allows a host VM to support multiple guests running on different operating systems.

* * *

### Swap Space
- Swap space in Linux is used when the amount of physical memory (RAM) is full

### Memory Page (a.k.a. Virtual Page)
- a fixed length contiguous block of virtual memory.
- each page is described by an record in a page table
