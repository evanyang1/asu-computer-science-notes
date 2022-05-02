# Module 4: Memory Management

## 4.1 Memory Management Fundamentals

Processes need memory for code, data, stack, heap. Code and data are static, stack and heap are dynamic. For stack and heap, some default amount of memory is allocated, i.e. 4 MB. If more memory is needed, need to call a system call. If not all the allocated memory is used, goes to waste (no one can use).

Contiguous memory allocation: no gaps, unless we're talking about unused memory in stack or heap, but no one can use that. First it's OS/RM memory, then P1 and P2 and so forth. So those gaps, we can call them **holes**, they're a problem. Yeah you can have a situation where there's enough memory available but it's not contiguous so a process may not be able to use it. Basically a process (in the middle of memory) finishes, and there's a hole. **external fragmentation**. Another type of external fragmentation is simply not enough memory out there.

**Internal fragmentation** = when there's unused memory b/c stack and heap didn't use all of the allocated memory block. No one else can use that memory. Inefficient

## 4.2 Solutions to Fragmentation

**Compaction**: a method to reclaim holes. Migrate processes from one memory location to another. This involves moving code: stack pointers, calling addresses, instruction pointers, heap contents. Remember that converting code to executable has many steps: compiling, dealing with tags and functions and linking, loading.
