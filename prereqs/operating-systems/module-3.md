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
+ **busy wait**
+ **spin-lock** - hardware support
```
test_and_set:
movl 4(%esp), %eax
movl 8(%esp), %edx
xchgl %eax, (%edx)
ret
```
+ advisory for Test_and_test: only for small and non conflicting critical sections, and only for uniprocessor systems.

### Semaphores

+ **Semaphores** (also called **monitors**): they are a data type, init value 0 or 1; 2 operations (P or V)
```
struct Semaphore {
  int value; // data
  function init(int x) {
    value = x;
  }
  function P(s){}
  function V(s){}
}
```
+ P(s):
```
if (S > 0) S--;
else goto (beginning of P(s))
// all this in atomic fashion
```
+ from above, if S == 0, it keeps blocking itself
+ V(s): `S++` in atomic way
+ P(s) is lock, V(s) is unlock
+ Semaphores easily solve mutex/critical section problem:: Init(S,1). Process 1: P(s), critical section, V(s). Process2: P(S), crit section, V(S).
+ When semaphores are used for mutual exclution -> **mutex semaphore**
+ You can also use semaphores for process synchronization. Like if you want T3 can only execute after T1 and T2, then use two semaphores, init to 1.
+ Careful, nested semaphores can result in **deadlock**.
+ semaphores can be used for speed control.

### Producer Consumer

[Google search](https://www.google.com/search?q=producer+consumer+bounded+buffer+problem&oq=producer+consumer+boun&aqs=chrome.0.0i512j69i57j0i512j0i390l3.3496j1j1&sourceid=chrome&ie=UTF-8)
+ Single producer and single consumer
+ Producer:
```
P(empty)
buff[in] = item
in = (in + 1) % N
V(full)
```
+ Consumer:
```
P(full)
Item = buff[out]
out = (out + 1) % N
V(empty)
```
+ For each of them above, infinite loop. P and V are semaphore functions.
+ For multiple producer/multiple consumer problem, we have race condition on in&out. Solution: mutex semaphore.
```
P(empty)  // Mutex = 1
P(mutex)
buff[in] = item
in++
V(mutex)
V(full)
```

### Dining Philosophers Problem

```
sem ch[5] = 1; // array of semaphores
Philosopher (i)
  loop
    THINK
    P(ch[i]); P(ch[i + 1] % 5); // right and left chopsticks, respectively
    EAT
    V(ch[i]); V(ch[i + 1] % 5);
```
+ Deadlock due to hold & wait, and circular wait
+ Solve by not doing hold & wait. Or don't do circular wait
+ Or if you don't get a right chopstick (for example), just let it go. Don't wait for the left (h&w)
+ Or have an extra chopstick

## 3.2 Concurrent Programming Algorithms

### Mutual Exclusion Critical Selection

+ Entry and exit functions must be the same for both threads, though it can be parametrized.
+ (Progress Property) If a thread wants to enter a critical section and another thread is executing non-critical section code, the 1st thread *must* be allowed to enter.
+ (Bounded Waiting) If T1 wants to enter a critical section and T2 is in a critical section, then we must guarantee that T1 will enter after T2 re-enters at most max # of times
```
turn = 0
// T1
while (turn != 0); // END LOOP
turn = 1;
critical section(CS);
turn = 0;

// T2
while (turn != 0); // END LOOP
turn = 1;
critical section(CS);
turn = 0;
```
+ Problem: what if T1 and T2 comes in at the same time?
+ While loop has a name, **busy waiting**.
```
// Second sol'n

turn = 0;

// T1
while (turn == 1);
C.S.
turn = 1;

// T2
while (turn == 0);
C.S.
turn = 0;
```
+ Problem: doesn't satisfy Progress property.
+ Solution with two bowls:
```
flags[2];

// T1
flags[0] = 1;
while(flags[1] == 1);
C.S.
flags[0] = 0;

// T2
flags[1] = 1;
while(flags[0] == 1);
C.S.
flags[1] = 0;
```

### Solutions

+ Dekker's Algorithm satisfies all three properties, for two processes. (1965)
```
flag[2] = {0, 0};
turn = 0; // turn can be initialized to be anything
// Thread 0
flag[0] = 1;
while (flag[1] == 1) {
  if (turn != 0) { // is it my turn to go?
    flag[0] = 0;
    while (turn != 0) {} // wait for its turn
    flag[0] = 1;
  }
}
// critical section
turn = 1;
flag[0] = 0;
// remainder section

// Thread 1
flag[1] = 1;
while (flag[0] == 1) {
  if (turn != 1) {
    flag[1] = 0;
    while (turn != 1) {}
    flag[1] = 1;
  }
}
// critical section
turn = 0;
flag[1] = 0;
// remainder section
```
+ all indices are thread id's, as well as turn values

### Peterson Algorithm (1981)

```
flag[2] = {0, 0};
turn = any (let's say 0);

// P0
flag[0] = 1;
turn = 1;
while (flag[1] == 1 && turn == 1)
  ;
// CS
flag[0] = 0;

// P1
flag[1] = 1;
turn = 0;
while (flag[0] == 1 && turn == 0)
  ;
// CS
flag[1] = 0;
```
+ beauty of this solution, is it uses race condition to solve race condition (r.c. occurs at turn = 1; and turn = 0;)

+ ***

+ N processes -> use Lamport's Bakery Algorithm
