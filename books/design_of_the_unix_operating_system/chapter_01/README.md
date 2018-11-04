

# General Overview of the system

## 1.2 System Structure

The hardware at the center of the diagram provides basic services to the operating system. The operating system interacts directly with the hardware, encapsulating the complexity between program/services and the hardware. It could be divided in layers such as image depicted below. The operating system lies on the `system kernel` or just `kernel` layer which is isolated from the `user` layer which is where user application run.

Program such as vim, ping, shell are shown in the outer layer, interact with the kernel through  a defined set of `system calls`. The system call instruct the kernel to do operations on behalf of the user process.

Private user programs may run on the system too, describes as `a.out`, as in the standard output for a C compiled (C Compiler or cc).

An standard C program invokes a C preprocessor, two-pass compiler, assembler, and loader (link-editor).

There are about 64 `system calls` in System V, which fewer than 32 are used frequently.

## 1.3 User's perspective

### 1.3.1 File System

The UNIX File system is characterized by:

- hierarchical structure
- consistent threatment of data files
- ability to create and delete files
- the protection of file data
- dynamic growth of files
- threatment of hardware devices (terminals, tape units, cdrom) as files


The file system is organised in a tree structure, with a single root leaf, called "root" or "/". Every non-leaf node in the structure can be a file, directory or device. The name of the file is given by a "full-path" name, that describes the location of this file.

For example: "/etc/passwd", "/usr/bin/who", "/bin/cat" and so forth.

Programs in UNIX do not know about the format of a file and it is thread it as an unformatted stream of bytes. Programs may interpret the byte stream in a different way (parse) but is not related about how the operating system stored the data. Thus the syntax to access a file is the same for all the programs but the format is imposed by the program.

For example the program "troff", expects a new line at the end of the file, but "acctcom" expects a fixed length in records but it's responsability of the application to apply the right format.


Permissions to access a file or directory are managed by the operating system. Permissions can be set to 'read', 'write' or 'execute' for 3 classes of users: 'owner', 'group' and 'others/everyone else'. Devices are protected in the same way as files are

For example consider the following example, which makes a copy of a given file. A user in the terminal can type:

```bash
copy oldfile newfile
```

where the oldfile is the existent file, and newfile the copy. Check the C code below:

```c++
#include <fcntl.h>
char buffer[2048];
int version = 1; /* Chapter 2 explains this */

main(argc, argv)
  int argc;
  char *argvt[];
{
  int fdold, fdnew;

  if (argc != 3) {
    printf("need 2 arguments for copy program\n');
    exit(1);
  }

  fdold=open(argv[1], O_RDONLY); /* open source file read only */

  if (fdold == —1) {
    printf("cannot open file %s\n", argv[1];
    exit(1);
  }

  fdnew creat(argv[2], 0666);
  if (fdnew == -1) {
    /* create target file rw for all */
    printf("cannot create file %An", argv[1];
    exit(1);
  }

  copy(fdold, fdnew);
  exit (0);
}

copy(old, new)
  int old, new;
{
  int count;
  while ((count = read(old, buffer, sizeof(buffer))) > 0)
    write(new, buffer, count);
}
```

- argc is 3 (number of arguments)
- argv[0] is the name of the program (copy)
- arg[1] is the first parameter (oldfile)
- arg[2] is the second parameter (newfile)
- all system calls return -1 on failure (error)

The program check first the number of arguments, if so it reads the file (open) with read-only mode, if the system call succeeded if will create a file (creat). The permissions on the new file will be set  to `0666`, allowing all users to read and write the file.


either file or device can be read it:

```
copy /dev/tty0 read-terminal.txt
```

which read the characters typed on the terminal  (/dev/tty0 is the user's terminal) and copy the content on `read-terminal.txt` file. Similarly,

```shell
copy /dev/tty0 /dev/tty0
```

reads the characters typed on the terminal and copies them back to user.


## 1.3.2 Processing Environment

A program is an executable file, and a process is an instance of the program in execution.
Many processes can be executed at the same time (multi-programming/multi-tasking), with no logical limit to their number and many instances of the program  (such as copy) can exist simultaneously in the system.

system calls allow process to create, terminate, synchronize stages of execution (?), control reactions of events, etc.

For instance, a process executing the following program, executes a `fork` system call to create a new process. The new process is called the `child` process and gets a return value of `0` from fork and invokes `execl` to execute  the program `copy` (the command from the previous section)

```c++
main(argc, argv)
int argc;
char *argv[];

/* assume 2 args: source file and target file */
if (fork() == 0)
  execl("copy, "copy", argv[1], argv[2], 0);

wait((int *) 0);
printf("copy done\n");
```

the `execl` call overlays the address space of the `child` process with the `copy` file (assuming to be in the same directory), and runs the program with the arguments given. If `execl` succeeds, it never returns because it is executed in a different address space (more in chapter 7).

While the process that invoked `fork` (parent process) receives a non-0 from the call, calls `wait`, suspending it's execution until `copy` finishes, prints "copy done" and exits (every program exits at the end of it's main function).


For example, if the executable name is `run` and the user invokes the program as:

```shell
run oldfile newfile
```

the process copies `oldfile` to `newfile` and prints the message. behind the scenes there are 4 system calls in place.

- `fork`
- `exec`
- `wait`
- `exit`(end of the execution)


The `shell` program, the interpreter you get once you logged in , translates a command line as a command name. for several commands, the shell `forks`, and the `child` process  `execs` the command associated with the name, treating the remaining words of the command line as parameters for the command.

```
$shell> ps
fork ->
  $shell child> exec ps
exit
```

The `shell` allows 3 types of commands:

- an executable binary (compiled)
- an executable file (sequence of shell commands)
- an internal shell command (for, while, if, elif, done)

The `shell` looks for commands in a given sequence of directories (`/bin:/usr/bin:/usr/local/sbin`). The shell usually executes a command synchroniously, waits for the command to finish before executing the next line. however, it also allows asynchronous execution (background).

For example:

```shell
who
```

Makes the system execute the program `who`, which is stored on `/bin/who` and prints the number of people connected to the system. During the execution, the shell waits for the results before an user can execute another command.

by typing:

```shell
who &
```

The system executes `who` in the `background` and the shell is ready to take another command.
Remember, the shell is a user program and not a kernel component.

## 1.3.3 Building block primitives

UNIX philosophy is to provide users primitives to write small modular programs that can be seen as blocks to build something more complex. One primitive visible to `shell` users is the capability to `redirect I/O`. Processes typically have access to three files:

- read from the standard input
- write to the standard output
- write error messages to the standard error file

A process executing in a terminal typically uses all of them, but they may be redirected independently. For example:

```shell
ls
```

list all the files from the current directory on the standard output, but the command line:

```shell
ls > output.txt
```

redirects the standard output to the file called `output.txt` in the current directory, using the `creat` system call. Similarly, the command line:

```shell
mail manuel < letter
```

opens the file "letter" for it's standard input and mails the content to the user named "manuel".
process can redirect input and output at the same time, for example:

```shell
nroff -mm < doc1 > doc1.out 2 > errors
```

the text editor `nroff` reads the input from the "doc1" file, redirects it's standard output to the file "doc1.out" and redirects error messages to the file "errors". The notation "2>" means to redirect the output for file descriptor 2 (conventially the standard error).

The shell recognizes the symbols ">", "<" and "2>" and sets up the standard input, output and standard error before executing a process.


The second building block primitive is the `pipe`, a mechanism that allows stream of data to be passed between the `reader` and `writer` process. A process can redirect it's standard output to a pipe to be read by another process that have redirected it's standard input to come from a pipe.
The data that the first processes write into the pipe is the input for the second processes. The second processes could also redirect their output and so on (depending on programming).

The processes need to know what type of file their standard output is, they work even if the standard output is a regular `file`, `pipe` or `device`.

For example, the program `grep` searches a set of files for an specific pattern:

```shell
grep main file1.c file2.c file3.c
```

searches for the word "main" in files "file1.c", "file2.c" and "file3.c". the output could be:

```bash
file1.c: main(argc, argv)
file2.c: /* here is the main loop in the program */
file3.c: main()
```

## 1.4 Operating system services

The figure depicts the kernel and user layer (add image here):


The kernel performs operations on behalf of user processes to support the user interface described above:

- Controlling execution of processes by allowing their creation, termination or suspension, and communication

- Scheduling processes fairly for execution on the CPU. Processes share the CPU in a `time-shared` fashion. The CPU executes a process, the kernel suspends it when it's `time quantum` elapses, and the kernel schedules another process to execute. The kernel later reschedules the suspended process.

- Allocating main memory for an executing process. The kernel allows processes to share portion of their memory address space under some conditions, but protects the private space of a process from outside tampering (other process cannot read address space from another process).

- If the kernel is low in memory, it frees memory by writing a process to a secondary memory called `swap`. If the kernel write the entire processes to a `swap` device, the implementation of the UNIX system is called `swapping system`; if it writes pages of memory to a real `swap` device it is called `paging system`.

- Allocating secondary memory for efficient storage and retrieval of user data (this is the filesystem). The kernel allocates secondary storage for user files, reclaims unused storage, structure the file system and protects the file from illegal access.

- Allowing processes controlled access to devices such as terminals, tape drives, disk drives, network cards, etc.

## 1.5 Assumptions about hardware

The execution of a user processes on UNIX is divided in two levels, user and kernel mode (system). When a process executes a `system call` the execution mode changes from `user mode` to `kernel mode`. The operating system executes the call and if it fails returns an error back to the user. Even if the user doesn't make any explicit requests for operating system services, the OS does `bookkeeping operations` related to the user process, handling interrupts, scheduling processes, managing memory, and so forth.

The main differences are:

- Processes in `user mode` can access their own instructions and data, but not `kernel` instructions and data. Processes in `kernel mode`, however, can access user and kernel user addresses.
For example the virtual address space of a process could be divided between addresses that are accessible only in kernel mode and addresses that are accessible in either mode.

- Some machine instructions are privileged and result in an error when executed in `user mode`. For example, a machine may contain an instruction that manipulates the processor status register; processes executing in user mode should not have this capability.

ADD FIGURE USER AND KERNEL PROCESS HERE

In summary, the hardware views the world in terms of user and kernel mode and doesn't know about users executing processes in those modes. the OS keeps an internal record about the processes running on the system. The previous figure shows the distinction, the kernel distinguishes between processes A,B,C and D on the horizontal axis, and the hardware distinguishes the mode of execution in the vertical axis.

Although the system executes in on of the two modes, the kernel runs on behalf of a user process. The kernel is not a separate set of processes that run in parallel to user processes, it is part of each user process. The text will referer to this as "the kernel allocating resources" or "the kernel" doing some operations, but what it meant is that a process running in kernel mode allocates the resources or does the various operations.

For example, the user `shell` reads from the terminal input via a `system call`, the kernel executing on behalf of the shell process, controls the operation of the terminal and returns the typed characters to the shell. The shell then executes in user mode, interprets the character stream typed by the user, does the specificied set of actions which may require invocations of other system calls.

## 1.5.1 Interrupts and Exceptions

UNIX systems allow devices such as I/O peripherals or the system `clock` to interrupt the CPU asynchronously. On receipt of the interrupt, the kernel saves it current `context` (a frozen image of what the process was doing), determines the cause of the interrupt, and then services the interrupt. After the kernel services the interrupt, it restores it's interrupted context and proceeds as if nothing happened.

The hardware prioritizes devices according to the priority. When the kernel receives an interrupt, it blocks out lower priority interrupts but services higher priority interrupts.

An `exception` condition referes to unexpected events caused by a process, such as addressing illegal memory, executing privileged instructions, dividing by `zero`, and so forth. Exceptions happen in the middle of the execution of an instruction, and the system attempts to restart the instruction after handling the exception; interrupts are considered to happen between the execution of two instructions, and the system continues with the next instruction after servicing the interrupt. UNIX systems use one mechanism to handle interrupts and exception conditions.


## 1.5.2 Processor execution levels

The kernel must sometimes prevent the ocurrence of interrumpts during critical activity, which could result in corrupted data if interrupts were allowed. For instance, the kernel may not want to receive a disk interrupt while manipulating `linked lists`, because handling the interrupt could corrupt the `pointers` (see next chapter).

Computers typically have a set of privileged instructions that set the processor execution level in the processor status word. Setting the processor execution level to certain values masks off interrupts from that level and lower levels, allowing only `high-levels` interrupts. Figure 1.6 shows a sample set of execution levels.
If the kernel masks out disk interrupts, all interrupts except for `clock` interrupts and machine error
interrupts are prevented. If it masks out software interrupts, `all` other interrupts may occur.



## 1.5.3 Memory Management

The kernel does live permanently on memory as does the currently executing process (part of it). When compiling a program, the compiler generates a set of addresses in the program that represent addresses of
`variables` and `data structures` or the addresses of `instructions` as functions.

the compiler generates the addresses for a `virtual machine` as if no other program will execute simultaneously on the physical machine.

when the program is ready to run, the kernel allocates space in main memory for it, but the `virtual addresses` generated by the compiler don't need to be identical to the `physical addresses`. The kernel coordinates with the machine hardware to setup a `virtual to physical address` translation that `maps` the compiled generated addresses to the physical machine addresses. That mapping depends of the machine capabilities, the parts of the UNIX system that deals with it are machine `dependent?`.

For example some machines have special hardware to support demand `paging` (more in chapter #6 and #9)


## Additional References

- NS3200 Micro-Controller https://en.wikipedia.org/wiki/NS320xx
- System V release 4 version 2 kernel https://archive.org/details/ATTUNIXSystemVRelease4Version2


