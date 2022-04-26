# Module 2: CPU Scheduling Concepts

## 2.0 Overview

+ CPU scheduling is a process which allows one process to use the CPU while the execution of another process is on hold(in waiting state) due to unavailability of any resource like I/O etc, thereby making full use of CPU. The aim of CPU scheduling is to make the system efficient.
+ Whenever the CPU becomes idle, the operating system must select one of the processes in the **ready queue** to be executed. The selection process is carried out by the CPU scheduler. The scheduler selects from among the processes in memory that are ready to execute, and allocates the CPU to one of them.

## 2.1 Processes and Programs

+ **Program** = group of instructions to carry out a specific task, **process** = a program in execution
+ Program is passive/static, process is active
+ Depending on the operating system (OS), a process may be made up of multiple threads of execution that execute instructions concurrently.
+ A program can be run by multiple processes (separate or concurrent)
+ A process can run multiple programs
+ A program can generate processes
+ Example of process: login process 
+ program written in high level language like C or Java; process is in something the computer can understand
+ Compiler converts something like C/Java to binary, but it's more complicated than that. For instance, compiler can't convert function code to binary, because not in memory. (We haven't assigned RAM to the program yet) Compiler doesn't know the first line of the program (address). To deal with functions, uses **relative reference** (tag) for that function. Another type of tag for library functions (compiler can't convert those either) (**symbolic reference**)
+ Output of compiler is binary plus some tags (called **object file**) The computer doesnt understand tags; object files (.o files) cannot be run. So the **linker** comes in and users a **relocation table** to convert tags to binary
+ Relocation table: table of pointers to lines of code that need linking to different libraries 
+ A **symbol table** is also created, which has info about the symbols, and what functions have to be loaded for a given symbol
+ Linker starts at address 0 then works its way thru the program 
