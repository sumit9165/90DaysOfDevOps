# Linux Operating System
## Operating System
Linux is an operating system like many others, such as DOS, VMS, OS/360, or CP/M. It performs many of the same tasks in very similar manners. It is the manager and administrator of all the system resources and facilities. Without it, nothing works.
## What is Linux?
Just like Windows, iOS, and Mac OS, Linux is an operating system.
An operating system is software that manages all of the hardware resources associated with your desktop or laptop.
The operating system manages the communication between your software and your hardware. Without the operating system (OS), the software wouldn’t function.
### Linux Operating System have several different pieces.
1. Bootloader –  The software that manages the boot process of your computer. For most users, this will simply be a splash screen that pops up and eventually goes away to boot into the operating system.
2. Kernel – This is the one piece of the whole that is actually called ‘Linux’. The kernel is the core of the system and manages the CPU, memory, and peripheral devices. The kernel is the lowest level of the OS.
3. Init system – This is a sub-system that bootstraps the user space and is charged with controlling daemons. One of the most widely used init systems is systemd. It is the init system that manages the boot process, once the initial booting is handed over from the bootloader (i.e., GRUB or GRand Unified Bootloader).
4. Daemons – These are background services (printing, sound, scheduling, etc.) that either start up during boot or after you log into the desktop.
5. Graphical server – This is the sub-system that displays the graphics on your monitor. It is commonly referred to as the X server or just X.
6. Desktop environment – This is the piece that the users actually interact with. Each desktop environment includes built-in applications (such as file managers, configuration tools, web browsers, and games).
7. Applications – For Example- Ubuntu Linux has the Ubuntu Software Center (a rebrand of GNOME Software) which allows you to quickly search among the thousands of apps and install them from one centralized location.
## Why Use Linux?
1. Linux is less vulnerable to such attacks. As for server reboots, they’re only necessary if the kernel is updated.
2.  It is not out of the ordinary for a Linux server to go years without being rebooted.
3.  If you follow the regular recommended updates, stability and dependability are practically assured.
4. You can install Linux on as many computers as you like without paying a cent for software or server licensing.
## Open Source
It’s about freedom and freedom of use and freedom of choice. Linux is also distributed under an open source license. Open source follows these key tenets:
1. The freedom to run the program, for any purpose.
2. The freedom to study how the program works, and change it to make it do what you wish.
3. The freedom to redistribute copies so you can help your neighbor.
4. The freedom to distribute copies of your modified versions to others.
## What is a “distribution?”
Linux has a number of different versions to suit From new users to hard-core users,you’ll find a “flavor” of Linux to match your needs.
These versions are called distributions (or, in the short form, “distros”).

Popular Linux distributions include:
1. LINUX MINT
2. MANJARO
3. DEBIAN
4. UBUNTU
5. ANTERGOS
6. SOLUS
7. FEDORA
8. ELEMENTARY OS
9. OPENSUSE
   
You can check out the top 100 distributions on the Distrowatch. And don’t think the server has been left behind. For this arena, you can turn to:
1. Red Hat Enterprise Linux
2. Ubuntu Server
3. Centos
4. SUSE Enterprise Linux
Some of the above server distributions are free (such as Ubuntu Server and CentOS) and some have an associated price (such as Red Hat Enterprise Linux and SUSE Enterprise Linux). Those with an associated price also include support.
## Which distribution is right for you?
Which distribution you use will depend on the answer to three simple questions:

1. How skilled of a computer user are you?
2. Do you prefer a modern or a standard desktop interface?
3. Server or desktop?
a. If your computer skills basic, you’ll want to stick with a newbie-friendly distribution such as Linux Mint, Ubuntu (Figure 3), Elementary OS or Deepin.
b. If your skill set extends into the above-average range, you could go with a distribution like Debian or Fedora.
c.  If, you’ve pretty much mastered the craft of computer and system administration, use a distribution like Gentoo.
d. If you really want a challenge, you can build your very own Linux distribution, with the help of Linux From Scratch.

## Installing Linux in Windows
1. Through WSL2.
2. Through Virtual Machines. 
3. Through DualBoot.
   
The Ubuntu Server does not install a GUI interface  
You can install a GUI package on top of the Ubuntu Server with a single command like 
--> sudo apt-get install ubuntu-desktop.
## Process & Process States:

A process is more than just a program. Especially in a multi-user, multi-tasking operating system such as UNIX, there is much more to consider. Each program has a set of data that it uses to do what it needs.This data is not part of the program. For example, if you are using a text editor, the file you are editing is not part of the program on disk, but is part of the process in memory. If someone else were to be using the same editor, both of you would be using the same program. However, each of you would have a different process in memory

Many different users can be on the system at the same time, they have processes that are in memory all at the same time. The system needs to keep track of what user is running what process, which terminal the process is running on, and what other resources the process has (such as open files). All of this is part of the process.
With the exception of the init process (PID 1) every process is the child of another process. Therefore every process with the exception of the init process has a “parent” process.
### Process States
The states that a Process enters in working from start till end are known as Process states. These are listed below as:
1. Created -Process is newly created by system call, is not ready to run
2. User running -Process is running in user mode which means it is a user process.
3. Kernel Running -Indicates process is a kernel process running in kernel mode.
4. Zombie- Process does not exist/ is terminated.
5. Preempted- When process runs from kernel to user mode, it is said to be preempted.
6. Ready to run in memory- It indicated that process has reached a state where it is ready to run in memory and is waiting for kernel to schedule it.
7. Ready to run, swapped - Process is ready to run but no empty main memory is present
8. Sleep, swapped- Process has been swapped to secondary storage and is at a blocked state.
9. Asleep in memory- Process is in memory(not swapped to secondary storage) but is in blocked state.

### After Process States status will be changing like this-
1. User-running: Process is in user-running.
2. Kernel-running: Process is allocated to kernel and hence, is in kernel mode.
3. Ready to run in memory: Further, after processing in main memory process is rescheduled to the Kernel.i.e.The process is not executing but is ready to run as soon as the kernel schedules it.
4. Asleep in memory: Process is sleeping but resides in main memory. It is waiting for the task to begin.
5. Ready to run, swapped: Process is ready to run and be swapped by the processor into main memory, thereby allowing kernel to schedule it for execution.
6. Sleep, Swapped: Process is in sleep state in secondary memory, making space for execution of other processes in main memory. It may resume once the task is fulfilled.
7. Pre-empted: Kernel preempts an on-going process for allocation of another process, while the first process is moving from kernel to user mode.
8. Created: Process is newly created but not running. This is the start state for all processes.
9. Zombie: Process has been executed thoroughly and exit call has been enabled. The process, thereby, no longer exists. But, it stores a statistical record for the process. This is the final state of all processes.

## Commands in daily use
1. pwd
2. ls
3. cd
4. mkdir
5. touch






