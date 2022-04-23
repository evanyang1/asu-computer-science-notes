# Module 1: Introduction to Operating Systems

## 1.0 Overview

+ Operating System (OS) = acts as intermediary between user and hardware
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
