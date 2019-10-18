# OS Protection Boundaries

- User mode: is where applications live (unprivileged mode)
- Kernel mode: is where the OS lives and performs the systems calls to interact with the hardware
- Crossing from user to kernel mode is supported by the hardware. There is a bit enabled on CPU hardware that grants this access

INSERT GRAPH HERE

- If a user mode tries to execute something that requires to be in kernel mode, it will cause a `trap`. The OS will check if the process
is granted to perform the operation.
- The interaction between the application and the system are through the `system call` interface. The OS exposes a set of operations that
are available on user mode to perform certain access such as `create`, `open` or `close` a file, create a socket `socket`, allocate memory with `mallac` and so forth.
- OS also supports `signals`, which allow the OS to interact with the process in user mode.

# System call flow chart

![system call flow](images/1_10_UserToKernelMode.jpg)


1. User executing a process in CPU
2. User executes a system call since needs hardware access
3. Application moves to kernel mode and executes the system call (`trap mode bit = 0`)
4. Moving from user to kernel mode generates a `context switch` (passing arguments and memory locations)
5. Once the system call is completed goes back to the `user mode` passing the return from the system call back (`return mode bit = 1`)


To make a system call an application must:

- write arguments (how many arguments to retrieve)
- save data at well defined locations (memory/disk)
- make a system call
- arguments can be passed through the application via `variables` or indirectly using their address.


NOTE: in sychronous mode the progress will wait until the `system call` is completed.


# References

- https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/1_Introduction.html