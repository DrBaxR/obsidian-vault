Tags: #reference 
Created: 2022-08-27 16:08

# Docker vs Virtual Machines
The operating system is split into two layers that go on top of the *hardware of a system*. These two layers are (bottom up):
- the OS [[Kernel]]
- the applications

A VM brings its own kernel and applications layer, which means that when you boot up a virtual machine, that virtual machine *does not use your kernel*, it uses its own.

Docker, on the other side, only brings its own applications layer, which means that when you run a docker container, that container uses *your kernel*.

Differences:
- containers are much smaller, since they don't implement the kernel layer
- containers are much faster to start
- while running VMs is 100% guaranteed to be compatible with any OS, the same thing is not true for containers. This is because let's say you have a Windows host and you want to run a Linux Docker image (application) on it. The Linux image may not be compatible with the Windows kernel, which means that it may not be able to be run on it.

## Resources
[[Docker Tutorial for Beginners]]