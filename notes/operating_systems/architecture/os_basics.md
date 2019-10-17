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

