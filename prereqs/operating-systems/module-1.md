# Module 1: Introduction to Operating Systems

## 1.0 Overview

+ **Operating System (OS)** = acts as intermediary between user and hardware
+ Purpose: Provide an environment where the user can execute programs in a convienient manner
+ Manages memory & processes, plus hardware & software. Allows you to communicate with computer w/o you needing to know computer's language.

## 1.1 Motivating Factors for Operating Systems

+ Address bus: one way (can only read addresses, cannot write addresses_
+ Data bus: two ways (can read and write to memory)
+ How to get a program into memory: 1. write program into memory manually (done in early days when we used microcontrollers) (waste of time and energy and money) 2. automate this process (OS does this)
+ The program that automatically loads programs into memory (from peripheral - maybe hard disk or CD drive) is called a **loader**. 
+ But who/what loads the loader? We don't wanna run into the same efficiency issue. That is the **bootstrap loader**. (= A loader that loads itself)
+ The bootstrap loader has a fixed part that is loaded into memory. This fixed part's role is to load the rest of the loader in the hard disk or CD into RAM.
+ Bootstrap loader is in **ROM** (Read Only Memory) so it can't be changed. 
+ The area where the rest of the bootstrap loader is stored in memory, is called the **boot block**.
+ Boot block loads the rest of the OS.
+ **Compiler** = converts human readable language (i.e. Java) into binary (More complex than this, but more on that later). It first converts the language into assembly language (machine readable)
+ There has to be a loader at every step
+ There is a need to delimit program steps. You don't want a compiler as input data in place of data input that's not supposed to be a compiler (there will be chaos).
+ **First motivation: Control program behavior: not eat up other (wrong/unintended) programs for input**
+ **Second: We can't let programs run infinitely long**
+ We need to stop programs so other programs can run

## 1.2 Interrupts

+ **Interrupts** are needed to fulfill the two motivating factors listed in 1.1
+ The CPU is always executing something (never sleeps). It is either executing your program, some background process, or an assembly level instruction called **NoOps**. 
+ We interact with CPU via mouse, keyboard, tapping screen (for touchscreens). All these are **events** (tell the CPU to stop and redirect). Interrupts help us do this. 
+ Interrupts: send square wave to int x line. Program counter stores the address of the next instruction you're gonna execute. Status register stores (more on this later). After the square wave, CPU pushes program counter onto the stack. Then searches for something called an **interrupt vector**. This IV stores the address of the program you want to run (after all that's the purpose of an interrupt). The program that you run after the interrupt is called is called the **interrupt service routine** (**interrupt handler**).   
+ There's a table in the OS that has a interrupt -> number -> interrupt vector (-> addr: interrupt service routine). Then push address into the program counter.
+ After the ISR is run, you return from the interrupt. Then the stack is popped -> into program counter. Basically return to previous program.
+ After the program counter is pushed onto the stack, the status register is pushed onto the stack. The status register has alot of stuff, but a very important part is the mode bit, which corresponds to user or privilege. This is related to one of the motivations of OS.

## 1.3 Resident Monitor Efficiency, Issues and Enforcement

