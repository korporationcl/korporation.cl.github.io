

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

```
copy oldfile newfile
```

where the oldfile is the existent file, and newfile the copy. Check the C code below:

```
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

```
copy /dev/tty0 /dev/tty0
```

reads the characters typed on the terminal and copies them back to user.


## 1.3.2 Processing Environment

A program is an executable file, and a process is an instance of the program in execution.
Many processes can be executed at the same time (multi-programming/multi-tasking), with no logical limit to their number and many instances of the program  (such as copy) can exist simultaneously in the system.

system calls allow process to create, terminate, synchronize stages of execution (?), control reactions of events, etc.

For instance, a process executing the following program, executes a `fork` system call to create a new process. The new process is called the `child` process and gets a return value of `0` from fork and invokes `execl` to execute  the program `copy` (the command from the previous section)

```
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

```
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

```
who
```

Makes the system execute the program `who`, which is stored on `/bin/who` and prints the number of people connected to the system. During the execution, the shell waits for the results before an user can execute another command.

by typing:

```
who &
```

The system executes `who` in the `background` and the shell is ready to take another command.
Remember, the shell is a user program and not a kernel component.

## 1.3.3 Building block primitives

UNIX philosophy is to provide users  primitives to write small modular programs that can be seen as blocks to build something more complex. One primitive visible to `shell` users is the capability to `redirect I/O`. Processes typically have access to three files:

- read from the standard input
- write to the standard output
- write error messages to the standard error file

A process executing in a terminal typically uses all of them, but they may be redirected independently. For example:

```
ls
```

list all the files from the current directory on the standard output, but the command line:

```
ls > output.txt
```

redirects the standard output to the file called `output.txt` in the current directory, using the `creat` system call. Similarly, the command line:

```
mail manuel < letter
```

opens the file "letter" for it's standard input and mails the content to the user named "manuel".
process can redirect input and output at the same time, for example:

```
nroff -mm < doc1 > doc1.out 2 > errors
```

the text editor `nroff` reads the input from the "doc1" file, redirects it's standard output to the file "doc1.out" and redirects error messages to the file "errors". The notation "2>" means to redirect the output for file descriptor 2 (conventially the standard error).

The shell recognizes the symbols ">", "<" and "2>" and sets up the standard input, output and standard error before executing a process.


The second building block primitive is the `pipe`, a mechanism that allows stream of data to be passed between the `reader` and `writer` process. A process can redirect it's standard output to a pipe to be read by another process that have redirected it's standard input to come from a pipe.
