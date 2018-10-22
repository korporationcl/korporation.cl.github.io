

# General Overview of the system

Unix became popular since it's inception in 1969, running on different types of machines such as microprocessors, mainframes. The system is divided in two parts. First part is related to programs and services (mail, shell, text editors, etc) and the second is the operating system that support those programs and services. This book is focus on the description of the UNIX System V, produced by AT&T but consider features from other systems too.

## History

In 1965 Bell Telephone laboratories joined efforts with General Electric and project MAC of the Massachusets Institute of technology to develop a new system. It was called "Multics". The goal, was to provide compute power and storage to a wide amount of users, so that they can share their data easily, if desired.

A primitive version of Multics was running on a GE 645 computer (1969), it did not provide the enough computing power, nor was clear it's development goals. Bell laboratories ended his participation on this project.

With the end of the Multics project, members from the Computing Science research center at Bell laboratories were left without a programming environment. In an attempt to provide a programming environment, Ken thompson and Dennis Ritchie (plus others), sketched a paper design for a file system that later evolved in the UNIX file system.

.....
//
// Not sure if continue on this
//
.....

## System Structure

The hardware at the center of the diagram provides basic services to the operating system. The operating system interacts directly with the hardware, encapsulating the complexity between program/services and the hardware. It could be divided in layers such as image depicted below. The operating system lies on the `system kernel` or just `kernel` layer which is isolated from the `user` layer which is where user application run.

Program such as vim, ping, shell are shown in the outer layer, interact with the kernel through  a defined set of `system calls`. The system call instruct the kernel to do operations on behalf of the user process.

Private user programs may run on the system too, describes as `a.out`, as in the standard output for a C compiled (C Compiler or cc).

An standard C program invokes a C preprocessor, two-pass compiler, assembler, and loader (link-editor).

There are about 64 `system calls` in System V, which fewer than 32 are used frequently.

## User's perspective

### File System

The UNIX File system is characterized by:

- hierarchical structure
- consistent threatment of data files
- ability to create and delete files
- the protection of file data
- dynamic growth of files
- threatment of hardware devices (terminals, tape units, cdrom) as files
