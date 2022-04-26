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
+ A **symbol table** is also created, which has info about the symbols, and what functions have to be loaded for a given symbol - which lines of code need modification
+ Linker starts at address 0 then works its way thru the program 
+ output of linker can't be read because it has no physical address. It's relative addresses, and relative addresses can't be run. Multiple programs will have the same relative address. But at this point, the only thing that needs to be done is convert relative address to physical address. Symbol table helps with that.
+ Output of linker is .exe file. Then give it to the **loader**, which invokes the memory management unit and get the actual physical memory for that program, store the program in physical memory, and updates relative addresses to physical addresses. Then it uses symbol table to convert function calls' relative addresses into physical addresses. 
+ Almost done! In order to execute the code, you need to start a process. Remember, a process has alot of stuff: registers, program counter, status register, stack, heap. Someone needs to update values to these registers; that process does that. OS starts the process. Loads the executable, loads the PC with the start address of `main()`.

### Process states

+ Process has these five states: Initial, READY, RUN, WAIT, EXIT
+ Initial: the process' status register, program counters, stack, etc... are being decided upon.
+ READY: the process' stuff are fixed, waiting to be run.
+ CPU schedule runs the process.
+ When a process is running, the program counter of CPU gets updated
+ What can happen to a process? 1. Ends. Goes to EXIT state and exits from system. 2. Wait for input/output. In this case, gets booted out from RUN state and goes into the WAIT state. Then when it gets the I/O it was waiting for, it cannot go directly back into the RUN state, must go back to READY state, where it awaits the CPU scheduler to pick it again. 3. Preemption (CPU scheduler stops the process and decides it's time for other processes to do their thing). The process goes back to WAIT state.
