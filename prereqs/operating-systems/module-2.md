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

## 2.2 Process Memory States (Context Switching)

+ Programs have two fixed parts – global data and code. Both global data and code need memory.
+ Stack - for function frames that is helpful for invoking functions and storing local variables, stack pointers, instruction pointers etc
+ Heap - dynamic memory allocation
+ Global data is static; both global data and code have fixed memory. Stack and heap are dynamic for memory
+ Fixed amount of memory allocated for both stack and heap. If more memory is needed, need system call to ask for more.
+ There needs to be demarcation of where stack ends & heap starts. -> Stack and heap pointers.
+ Program counter stores address of next instruction. Stored in instruction register, or EIP.
+ EBP/Base register stores the base (from base and bound, memory protection)
+ While we switch processes, we can't lose the values of the process (i.e. base, program counter). Solution: **process control block**. One PCB per process. PCB stored in kernel.
+ PCB contains memory image (data, code, stack, heap), CPU registers (PC, stack pointer), OS info about process (process id, priority, ...)
+ PCB can't be stored in RAM where it can be written by other programs.
+ context switch is nothing but an interrupt. Use interrupt to track time, then when the specified time is hit, invoke context switch interrupt routine.
+ context switch manipulates stack pointer in order for the instruction pointer to point at the address of the new process 

## 2.3: CPU Scheduling (Non-Preemptive Scheduling)

+ **Scheduling queues**
+ There's only a single process in the RUN queue. Whereas can be >1 in other queues like the I/O wait queues.
+ Upon exit, a process needs to free up unused memory, and clean up the stack.
+ Non preemptive scheduler: Rarely used in general purpose computing systems but used in embedded system and other smaller different types of computing systems. CPU cannot stop a process while it's running. 
+ Preemptive scheduler: CPU can stop a process while it's running.
+ **burst** = collection of CPU cycles. **I/O burst** = time doing I/O. A process consists of CPU and I/O bursts. A process always starts and ends with a CPU burst.
+ During I/O burst, CPU is idle. We can run other processes.
+ **CPU Bound Process** = when a process spends more time doing CPU bursts than I/O bursts (**I/O Bound Process** vice versa)
+ CPU scheduling algorithms: First in first out (FIFO): two inefficiencies (system idle time and process wait time)

## 2.4 CPU Scheduling Metrics

+ **CPU Utilization** = % time CPU not idle = (total time - idle time)/total time, total time also called **makespan**
+ **Turnaround Time** = average time for a program or process to finish (including call delays). want this low
+ we compare cpu scheduling algorithms based on average turnaround time
+ **waiting time** = amount of time a process waits in waiting queue (want this low)
+ **response time** = amount of time from submission to its first running 
+ **throughput** = # processes completed per time unit
+ **service time** = The amount of CPU time that a process will need before it either finishes or voluntarily exits the CPU, such as to wait for input / output.
