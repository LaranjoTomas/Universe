
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

A resource is something a process needs to access. They may either be **physical components of the computational system** (processors, regions of the main or mass memory), or **common data structures** defined at the operating system level (process control table, communication channels) or among processes of an aplication.
**Resources** are devided in:
- **Preemptable resources -** when they can be taken away from the processes that hold them without causing malfunction; 
- **non-preemptable resources -** When it is not possible to do the above; 
## Deadlock characterization

In a **deadlock situation**, only **non-preemptable** resources are relevant. The remaining can always be taken away, if necessary, from the processes that hold them and assigned to others to ensure that the latter may progress.
![[deadlock_charc.png]]**Necessary conditions for Deadlock**
Deadlock can only occur if **all four conditions** below hold simultaneously:
1. **Mutual Exclusion:** Each resource is either free or assigned to exactly one process
2. **Hold and wait (waiting with Retention):** A process holding some resources can request additional resource without releasing the ones it holds
3. **No Preemption (Non-Liberation):** Resources can only be released voluntarily by the process holding them
4. **Circular Wait:** A cycle exists where each process waits for a resource held buy the next process in the chain

### Deadlock prevention

Mutual exclusion is necessary for non-preemptable resources to avoid race conditions and data inconsistency. However, it can be too strict for some cases, like reading a file, where multiple processes can safely read simultaneously. Often, systems allow many readers but only one writer at a time. Despite this, race conditions and data loss risks during writing cannot be fully eliminated.

### Denying the Condition of Waiting with Retention
A process must request **all the resources it needs at once** to continue. If it obtains all of them, the process can complete its activity; otherwise, it must wait. However, this approach does **not prevent indefinite postponement**. To address this, aging policies are commonly used to increase the priority of waiting processes, ensuring eventual access to resources

### Imposing the Condition of Liberation of Resources

If a process cannot acquire all the needed resources, it must **release all resources currently held** and restart the request procedure later. Alternatively, a process may be restricted to holding only one resource at a time, though this is rarely practical.

Processes should **avoid busy waiting**; after releasing resources, they should block and only be awakened when the required resources become available.

Indefinite postponement remains possible, so aging policies are again used to promote fairness.

### Denying the Condition of Circular Waiting

To prevent circular waiting, resources are given a **linear ordering**. Processes must request resources in **increasing order** of their assigned numbers.

This ordering prevents the formation of circular wait chains among processes.

Despite this, indefinite postponement can still occur, so aging policies to increase process priority are commonly implemented.

## Monitors

An application written in a concurrent language, implementing the shared variables paradigm, is seen as a set of threads that compete for access to shared data structures. When the data structures are implemented as monitors, the programming language ensures that the execution of a monitor primitive is carried out following a **mutual exclusion discipline**. Thus, the compiler generates the necessary code to enforce this condition transparently to the application programmer.

A thread enters a monitor by calling one of its primitives, which is the only way to access the internal data structure. Since primitive execution entails mutual exclusion, if another thread is already inside the monitor, the calling thread is **blocked at the entrance**, waiting for its turn.

Synchronization among threads using monitors is managed by **condition variables**. Condition variables are special constructs defined inside a monitor, where a thread may be blocked while waiting for an event that allows it to proceed. There are two atomic operations on a condition variable:

- **wait** – the calling thread blocks on the condition variable and is placed outside the monitor, allowing another thread to enter.
- **signal** – if there are threads blocked on the condition variable, one is woken up; otherwise, nothing happens.

To prevent multiple threads from coexisting inside a monitor, rules are needed to resolve contention when a **signal** operation occurs. Common approaches include:

- **Hoare Monitor**: The thread calling `signal` is suspended outside the monitor so that the woken thread can proceed immediately. This is a general solution but requires a stack to store suspended signal-calling threads.
- ![[Hoare_monitor.png]]
- **Brinch Hansen Monitor**: The thread calling `signal` must exit the monitor immediately (signal should be the last instruction in any access primitive, except for a return). This approach is simpler but more restrictive, limiting signal calls to one per primitive.
![[Brinch_monitor.png]]
- **Lampson/Redell Monitor**: The thread calling `signal` continues execution, while the woken thread remains outside the monitor and must compete again for access. This is simple to implement but can cause indefinite postponement of some threads.
![[Lampson_monitor.png]]
# Introductory Concepts

# System models

## Models in Distributed Systems

- **Architectural models:** they define the way how the different components of the system are mapped into the nodes of the underlying parallel platform and how they interact among themselves
	- **mapping wise:** goal is to make effiencien patterns of data distribution and processing loads
	- **interaction wise:** describe the functional role of each component and their communication
- **Fundamental models:** they discuss the systemic characteristics affecting dependability
	- **interaction type:** solve issues with communication bandwidth and latency
	- **failure handling:** specify the kind of failures that may occur
	- **security:** thrreats affecting system performance

## Client-server model

When processes and shared resources are migrated to different computer systems, their addressing spaces become separate. Communication between them must happen—either implicitly or explicitly—via message passing over a common communication channel.

A key point is that the shared resource is passive. To manage access, a new process is created whose job is not only to communicate with other processes but also to execute operations locally on the shared resource.

This leads to an operational model where resource manipulation is understood as **service rendering**. The process managing the shared resource locally is called the **server**, while the other processes that want access are called **clients**.

Operations on the shared resource can be divided into three phases:
- **Request**: The client process calls the server process, asking it to perform an operation on its behalf.
- **Local Execution**: The server process executes the requested operation on the shared resource.
- **Reply**: The server process sends the result back to the client process.
![[Client-Server_1.png]]

The server process typically has two roles:
- **Communication Manager**: Waits for service requests from clients.
- **Service Proxy Agent**: Executes operations on the shared resource as a proxy for the clients.

The communication model is **asymmetric**:
- **Server → public / Clients → private**: The service must be publicly known to all interested parties, but the service users (clients) do not need to be known in advance by the service providers (servers).
- **Server → eternal / Clients → mortals**: The server is expected to be permanently available, while clients use the service sporadically.
- **Centralized control**: All service operations are managed by a single centralized entity.

## Basic Server Architecture

Role of the Base Thread
- Instantiate the shared resource.
- Instantiate the communication channel and map it to a publicly known address.
- Start listening on the communication channel.
- When a connection from a client process is established, create a **service proxy agent thread** to handle the client’s request.

Role of the Service Proxy Agent Thread
- Determine the operation the client process wants performed on the shared resource (**request**).
- Execute the requested operation on behalf of the client (**local execution**).
- Communicate the result of the operation back to the client process (**reply**).
## Variants of the client-server model

### Type 1 Variant: Single Client at a Time

![[request_serialization.png]]
- Only one client process is serviced at a time.
- The base thread, upon receiving a connection request, creates a service proxy agent and waits for its termination before listening again.
- Since there is a single active service proxy agent thread, the shared resource requires no special mutual exclusion protection.
- This model is simple but inefficient because:
    - Service time is not minimized, as it doesn’t utilize the dead times during client interaction.
    - It causes busy waiting when multiple clients try to synchronize on the shared resource.
### Type 2 Variant: Server Replication
![[server_replication.png]]
- Multiple client processes are serviced concurrently.
- The base thread creates a service proxy agent for each new connection and immediately resumes listening.
- The shared resource is transformed into a **monitor** to ensure mutual exclusion because multiple service proxy threads can be active simultaneously.
- This is the traditional server setup, designed to fully utilize system resources.
- Advantages include:
    - Minimized service time by leveraging concurrency.
    - Enables synchronization of different clients on the same shared resource.

### Type 3 Variant: Resource Replication
![[resource_replication.png]]
- The service is available simultaneously on several computers, each running a type 2 (server replication) variant of the server.
- The shared resource is **replicated** across these servers, creating multiple copies.
- This advanced model aims to maximize service availability and minimize service time, especially during peak loads, enhancing scalability.
- Features:
    - Service remains operational even if some servers fail.
    - Client requests are distributed using DNS-based policies:
        - Geographical association for global requests.
        - Rotational association for local requests.
    - Consistency management is required to keep replicas synchronized when local data changes.

## Peer Communication

![[peer_communication_SD.png]]
- The service is simultaneously provided by multiple computer systems, each acting as an equal **peer**—there is no hierarchy among them regarding the service.
- The shared resource is replicated across all peers, resulting in multiple copies.
- This sophisticated model aims to maximize service availability and minimize service time, especially during peak loads, thus enhancing scalability.
- Key features:
    - The service remains operational even if some peers fail.
    - Occasionally, one peer assumes a **leading role** to help the system transition smoothly between stable states.
    - The leader is chosen through an **election process**, where consensus among peers must be reached.

## Communication primitives

Communication through message passing involves two main entities:
- **Forwarder:** the sender of the message
- **Recipient:** the receiver of the message

The **send** primitive requires at least two parameters:
1. Destination address
2. Reference to a user-space buffer containing the data to be sent
The **receive** primitive also requires at least two parameters:
3. Source address
4. Reference to a user-space buffer where the received data will be stored

Typically, communication is **buffered by the operating system**:
- On send, data is first copied into a kernel buffer before being sent over the network
- On receive, data is first stored in a kernel buffer and only transferred to the user buffer when the receive primitive is called

Communication primitives can be classified based on coupling and blocking behavior:
### Coupling types:
- **Synchronous:**
    - Send and receive are coupled
    - The send operation completes only when the recipient confirms the receive operation finished
- **Asynchronous:**
    - Send and receive operate independently
    - The send operation completes as soon as data is copied from the user buffer
    - There is no asynchronous receive primitive
### Blocking types:
- **Blocking:**
    - Control returns to the invoking process only after the communication operation completes (synchronous or asynchronous)
- **Non-blocking:**
    - Control returns immediately after the primitive is called, even if the operation is not yet completed

## Publisher-subscriber model

In the publisher-subscriber model, multiple service providers (**publishers**) and multiple service recipients (**subscribers**) are completely decoupled by an intermediary service called the **broker**.

- **Publishers:**  
    Produce information on different **topics** and act as clients of the broker. The broker stores this information and makes it available to subscriber groups who have explicitly subscribed to those topics.
- **Subscribers:**  
    Act as clients of both publishers and the broker:
    - To publishers, as consumers of the specific information they produce
    - To the broker, to inform which topics they are interested in and to receive alerts when new data on those topics become available
![[publisher-subscriber_model.png]]

Unlike the traditional client-server model, the publisher-subscriber model involves **no synchronous interaction** between publishers and subscribers.

### Electronic Trading (e-trading)

**Sellers** act as **publishers**, posting data about products they want to sell on a trading platform.
**Buyers** act as **subscribers**, requesting information about the products they want to buy.
**Trading platform** is the **broker** who manages possible transactions among them.
![[e-trading.png]]

### Electronic Mail (e-mail)

- Mail servers act as **brokers** for local mail clients and other mail servers (managing local messages), and as **publishers** for forwarding remote messages.
- Mail clients act as both **publishers** (sending messages) and **subscribers** (receiving messages).
- Buyers act as **subscribers** requesting product information from the trading platform.
- The trading platform itself acts as the **broker**, managing transactions between sellers and buyers.
- 