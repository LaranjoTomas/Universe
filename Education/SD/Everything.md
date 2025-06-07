
# Concurrency

## Program vs Process

Program is a sequence of intrusctions which describe the execution of a task in a pc. 
A **Program** execution is called a **process**. 
**Process** is characterized by:
- The **Addressing space** - the code and values of all variables
- the **processor context** - Value of all processors internal registers
- the **I/O Context** - all data transferred to the input and from the output 
- the **state of the execution**

**Multiprogramming** creates the illusion of multiple programs running simultaneously on a single processor. 
In a simplefied model: 
- The execution of a process is unaffected by when or where context switches happen
- There are no constraints on how long a process runs

![[state_diagram_sd_1.png]]
The **Scheduler** (processor scheduler) is the part of the operating system resposible for managing process state transitions. It is a core component of the kernel:
- **Exception Handling**
- **Processor allocation**
- **Assignment of system resources t o processes**
All together, these functions ensure efficient execution and coordination of processes within the system.

## Processes vs. Threads
A **process** has two main properties:
- **Resource ownership** - includes its own memory space and communication channels
- **Thread of execution** - Includes a program counter, CPU registers, and a stack(keeps history of execution)
These two aspects can be separated:
- A **process** may own resources
- A **Thread (light weight processes)** is the actual runnable unit
In **Multithreading**, multiple threads run independently **within the same process**, sharing resources but having their own execution state. This allows concurrent execution and efficient resouce use.

## Multithreading 
### Advantages

1. **Simpler Design and modularity**
	- Easier to design and implement programs with multiple concurrent activities or services
	- Promotes modular, decomposed solutions compared to sequential designs
2. **Better Resource Management**
	- Threads share the same memory space and I/O context within process
	- Simplifies memory and I/O management
3. **Higher Efficiency and Performance**
	- Threads require fewer OS resources than full processes
	- Operations like creation, termination, and context switching are faster
	- In symmetric multiprocessing systems, multiple threads can run in parallel, improves execution speed

### Organization
![[Organization_multiprocess.png]]
- Each **thread** usually runs a specific function or procedure representing an independent activity
- All threads within a process **share a common global data space**, including variables and I/O communication channels. **Allows shared read and write access**
- The **main program** acts as the **initial thread**, created first and typically the last to finish

### Support for Multithreaded Environments

1. **User-level Threads**
	- managed by a **user-space libray**
	- **Advantages:** Portable and flexible
	- **Disadvantages:** If one thread makes a **blocking system call**, the entire process is blocked
2. **Kernel-Level Thread**
	- managed directly by the **operating system kernel**
	- **Advantages:** Allows **true concurrency**
	- **Disadvantages:** 

## Test Preemptiveness

![[Test_preemptiveness_1.png]]

There are **4 sets**, each with: 
- 1 **computation-intensive thread**
- 1 **I/O-intensive thread**
- 1 **shared variable**
**Computation-Intesive Thread** increments the shared variable **10 million times**, one by one,
**I/O-intensive thread** continuously **reads and prints** the shared variable until it reaches 10 million.
**Thread priorities** can be adjusted, a **yield** (pause to allow other threads to run) may be added after each increment
The **Number of reads/prints** performed bu the I/O thread is used to **estimate the relative execution speed** between the 2 threads.
![[Test_preemptiveness_2.png]]

## General Principles

In multiprogrammed environments, processes may differ by behaviours:
- **independent processes -** Created, executed, and terminated **without direct interaction**. Interaction is **implicit**, arising from **competition for system resources**. It is the OS responsibilty to ensure that the resource assignment is carried out and organized. Requires a single process has access at a time to the resources
- ![[Indep_process.png]]
- **Cooperating Processes -** **Explicitly share or communicate** during execution. It is the involved processes responsibility to ensure access to the shared region is carried out in a controlled way. **Sharing** requires **common address space**, while **communication** can occur throught:
	- Shared memory
	- Communication channels
- ![[cooperating_process.png]]

When a **process** accesses a resource or shared region, it is actually the processor executing the relevant code.
This access code is called a **critical region** because:
- It must be executed **safely**, without interruption
- Improper handling can cause **race conditions**, leading to **data loss or inconsistency**.
![[Critical_region_SD.png]]

Imposing mutal exclusion or access to a resource may have 2 undesirable consequences:
- **deadlock/livelock -** happens when two or more processes are waiting forever (blocked / in **busy waiting**) for access to the respective critical regions, as a result the operation cannot proceed
- **indefinite postponement -** happens when 1 or more processes compete for the access to a **critical region** due to:
	- Constant arrival of new competing processes
	- Circumstances that **favor other processes** repeatedly

## Problem of access to a critical region with mutual exclusion

Desirable properties for **access to a critical region** must guarantee:
- **Mutual Exclusion:** Only **one process** can access the critical region at any given time
- **Independence from Process Speed or Number:** The solution must work regardless of how fast processes run or how many there are.
- **No Blocking by outside Processes:** A process **outside** the critical region **cannot prevent** others from entering
- **No indefinite Postponement:** Every process that wants access will eventually be allowed
- **Finite Time in Critical Region:** A process spends a **limited, finite amount of time** inside the critical region

## Resources

A resource is something a process needs to access. They may either be **physical components of the computational system** 