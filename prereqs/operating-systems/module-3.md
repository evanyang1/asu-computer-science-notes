# Module 3: Concurrent Processes

## 3.1 Hardware Based Conditions

### Race Conditions in Threads

+ **Race condition** - inconsistency due to parallelism.
+ two threads share code, global data, and heap, but they each have their own stack. Variable changes happen in stack. => problems!
+ Read-write or write-read conflict - one thread reads a variable that another thread is writing to
+ Write-write conflict - two threads try to write to same variable & we don't know what would be the final value of that variable
+ `x = x + 1;` isn't a single statement, it's a collection of 3 statements!
```
load x -> R1
add 1 to R1
store R1 -> x
```
+ that's how two threads doing `x = x + 1` can lead to only one increment instead of two
+ race conditions can happen in kernel threads too. Race conditions are an OS issue, not a coding issue (otherwise they wouldn't be discussed in this class)
+ Even when kernel threads don't have anything shared, this problem can still happen. Two programs make the same system call (affect global data).
+ **Linearizability** = whether a race condition is linearizable or not (linearizable = easier to fix), whether a serial pathway can be deduced or not
+ Prevent race conditions: Hardware based solutions - atomicity, software based - mutex, critical sections

### Railway in the Andes

+ Story representing the race condition/shared variable problem
+ Very difficult problem

### Hardware Solutions to this problem

+ One solution is stop context switching, disable interrupts (Used in practice)
+ There's a system call that says, disable all system interrupts
+ Only works for uniprocessor system
+ interrupt disabling must be a privileged intruction. This is a very dangerous operation. Every keyboard stroke, every mouseclick triggers an interrupt. You don't want your entire system to be unresponsive. Interrupt disabling code must be a very short segment of code. And this doesn't work for multi-processor system.
+ Multicore systems use atomic instructions. Test and set (discussed in this course), compare and swap (not discussed in this course), exchange (an implementation of test and set, used by Intel processors)
```
int testAndSet(&x) {
  temp = *x;
  *x = 1;
  return (temp);
}
```
+ **atomic** = during the execution of these lines of code, can't context switch
+ `while ( testAndSet(x) == 1)` solves critical section problem.
 
