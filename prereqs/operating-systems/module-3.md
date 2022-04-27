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
