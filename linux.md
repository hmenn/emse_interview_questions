## Linux specific questions that I've met

1. What happen when user type a command into terminal/bash?
    - Bash utility will fork itself and child process will change it's content with execv command. New content could another utility like ls, ps or user written executable.
2. Please describe monolithic, micro, modular kernel types? What is the type of Linux among them?
    - monolithic: There are one address space and one process that control operations.
        - Monolithic kernels hold all code within the kernel
        - Unix, Linux, Open VMS, XTS-400 etc.
    - micro: Only contains basic functionalities. User space and kernel space are separated.
        - Mach, L4, AmigaOS, Minix, K42 etc.
    - modular: A modular kernel allows an administrator to add functionality only when required
    - hybrid: It is the combination of both monolithic kernel and mircrokernel. It has speed and design of monolithic kernel and modularity and stability of microkernel.
        - Windows NT, Netware, BeOS etc.
    * Linux Kernel is a monolithic kernel, but most flavours of Linux such as Ubuntu, Solaris, use a hybrid kernel, i.e. a mix of the monolithic and modular kernel approach.
