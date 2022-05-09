# Module 4: Memory Management

## 4.1 Memory Management Fundamentals

Processes need memory for code, data, stack, heap. Code and data are static, stack and heap are dynamic. For stack and heap, some default amount of memory is allocated, i.e. 4 MB. If more memory is needed, need to call a system call. If not all the allocated memory is used, goes to waste (no one can use).

Contiguous memory allocation: no gaps, unless we're talking about unused memory in stack or heap, but no one can use that. First it's OS/RM memory, then P1 and P2 and so forth. So those gaps, we can call them **holes**, they're a problem. Yeah you can have a situation where there's enough memory available but it's not contiguous so a process may not be able to use it. Basically a process (in the middle of memory) finishes, and there's a hole. **external fragmentation**. Another type of external fragmentation is simply not enough memory out there.

**Internal fragmentation** = when there's unused memory b/c stack and heap didn't use all of the allocated memory block. No one else can use that memory. Inefficient

## 4.2 Solutions to Fragmentation

**Compaction**: a method to reclaim holes. Migrate processes from one memory location to another. This involves moving code: stack pointers, calling addresses, instruction pointers, heap contents. Remember that converting code to executable has many steps: compiling, dealing with tags and functions and linking, loading. This is so time inefficient! Potential recompile and restart of program.
###### Policies to improve efficiency
One way is to reduce program size. It reduces need to migrate. Might even finish before considering migrating the code. Even if you do need to migrate, fewer changes needed.
+ Dynamic linking & loading - do them on demand instead of at start of the program
+ Overlays - we fragment the program. Load one fragment at a time. Overlay the next fragment over the current one. Advantage: low program size. Disadvantage: Fragment swapping is expensive. What if the program switches between fragments? Also for loops are problematic, you might need two fragments at once but you can't -> infinite cycle.
+ Swapping - put program P1 onto hard disk, replace with P2
+ Memory Allocation Protocol - first fit, best fit, worst fit, etc... and hope for the best. But can't prove which one of the three works best in a given scenario.

## 4.3 Paging

+ **Paging** - a memory management scheme that eliminates the need for contiguous allocation of physical memory. Permits the physical address space of a process to be non-contiguous. Divide memory into fixed sized **frames**. Set a frame size, S.
+ at least S numbers to devote to S lines. log<sub>2</sub> S number of bits
+ line number log<sub>2</sub> S, frame number 32 - log<sub>2</sub> S
+ (RAM is also called physical memory)
+ Program is divided into **pages**, page size set to be equal to frame size.
+ Last page will never be full -> unused memory. Still called internal fragmentation.
+ Then allocate page to frame.
1. No need for order
2. No need for contiguous allocation.
+ To make sure execution is correct, we have frame number + line number (aka offset).
+ Relative address: for each page, the relative address resets
+ Given a line number, page number = (line number)/(page size = frame size) integer division, line number = offset = (line number) % (page size)
+ The MMU (Memory Management Unit) uses a **page table** to convert logical addresss to physical address
+ each entry in the table called a **page table entry (PTE)**. Maps logical page number to physical frame number
+ **frame sharing** - processes can share frames. good because more processes with smaller memory.
+ Page table stored in kernel memory (RAM)
+ Disadvantages: paging is twice the time because gotta access page table. paging time = (time to access page table) + (time to access physical address)
+ *solution* - decrease time to access page table. We can do this by using a **Translation Lookaside Buffer (TLB) Cache** -> Makes time to access table almost zero!
+ However because each process has its own page table and 100s of processes means a bunch of tables, we can't put them all in the cache, which is really small! TLB Cache miss => 3 memory accesses aka worse than the original paging scheme.
+ **Hit ratio** = (# cache hits)/(# cache queries)
