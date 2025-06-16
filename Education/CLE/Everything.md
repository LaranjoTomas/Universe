# Concurrency

## Computer Architecture
![[image 4.png]]

A computer system comprises several interrelated components that work together to process and manage data:
### Main Components
- **Processor (CPU)**: Controls operations and executes data processing tasks.
- **Main Memory**: Temporary and **volatile** storage for data and instructions during execution.
- **Mass Storage**: **Non-volatile** storage used to retain data between runs; also functions as a specialized I/O device.
- **Input/Output (I/O)**: Manages communication with external devices (e.g., keyboard, disk, network).
- **System Interconnect**: Typically a **bus**, it enables communication between all internal components.

## Stored-Program Concept
Modern computing is based on the **stored-program architecture**, independently developed by **John von Neumann** and **Alan Turing**.
In this model, both **instructions and data** are stored in main memory, making instructions a special kind of data.
This allows:
- Programs to be **loaded dynamically**.
- Programs to be **modified and executed** without changes to the hardware.
This principle underlies what we call **von Neumann machines**.

## Execution Cycle
A computer operates by **alternating between two core states**:
1. **Instruction Fetch**: The processor retrieves the next instruction from memory.
2. **Instruction Execution**: The processor decodes and executes the instruction.
This cycle forms the **foundation of all program execution**.

## Internal Structure of a Processor
The processor can be broken down into:
- **Control Unit**: Handles instruction fetching and decoding.
- **Arithmetic/Logic Unit (ALU)**: Executes logical and arithmetic operations.
- **Register Bank**: Temporary high-speed storage for quick data access.
- **I/O Buffers**: Interface to exchange data with other system components.
## Pipelining
**Pipelining** is a key technique to enhance performance:
It breaks down a complex task into a sequence of **independent stages** that work in parallel on different inputs.
- Each stage (called a **pipeline stage**) processes a specific portion of the task.
- Stages operate **simultaneously** on different data items.
- The result is **increased throughput**, as new tasks enter the pipeline before previous ones are fully completed.
![[image-1 1.png]]
This is foundational for **high-performance processors** and parallel systems.
### Speedup from Pipelining
The speedup **S** gained from pipelining is defined as:
$$S = \frac{t_{non-pipeline}}{t_{pipeline}} $$
Where:
- $t_{non-pipeline} = m * t_{cycle}$ 
- $t_{pipeline} = (m + (N-1)) * t_{cycle}$
with:
- **m**: Number of objects in the task stream
- **N**: Number of pipeline stages (subtasks)
- $t_{cycle}:$ Clock cycle time

**Ideal Case**: If **m**≫**N**, the speedup approximates:
$$S ≈ N$$
Thus, a pipeline with **N** stages ideally gives an **N-fold speedup**.

## Instruction-Level Parallelism (ILP)
**ILP** allows multiple instructions to be processed **simultaneously** by dividing the instruction execution into independent subtasks.
### Key Techniques Enabling ILP
- **Multiple-Issue**: Launches multiple independent instructions in the same cycle.
- **Pipelining**: Enables concurrent execution of instruction segments across different stages.
- **Branch Prediction & Speculative Execution**:
    - Predicts the outcome of conditional instructions.
    - Executes predicted paths ahead of time to **reduce stalls**.
- **Out-of-Order Execution**:
    - Rearranges instructions for **optimal scheduling**, while preserving correctness.
- **Prefetching**:
    - **Proactively loads data** into cache/memory before it’s needed, reducing latency.

## Cache Coherence in Multicore Systems

In **multicore processors**, multiple cores may access and modify **shared data** concurrently. This introduces the challenge of maintaining **cache coherence**, especially when data is copied into different cache levels (L1, L2) across cores.
### The Coherence Problem
- Copies of the **same memory block** may exist in multiple **L1 or L2 caches**.
- If one core modifies its copy, other cores may read **stale data** unless coherence is maintained.
### Ensuring Coherence
- Implement a **write-through policy** for L1 and L2 caches.
- Upon write, all copies in L1/L2 caches must be **invalidated**.
- Future reads then fetch the **updated block from L3**, which is treated as the most up-to-date source.

## Program vs. Process
- A **program** is a _static_ sequence of instructions.
- A **process** is a _dynamic_ execution instance of a program.
### 🔍 A Process Includes:
- **Addressing space** – Code and current variable values.
- **Processor context** – Register states.
- **I/O context** – Data in I/O transactions.
- **Execution state** – Current phase in execution.
## Process States and Scheduling

### Main States of a Process:
- **Running** – Actively using the CPU.
- **Ready** – Waiting to be scheduled on the CPU.
- **Blocked** – Waiting for an external event (e.g. I/O).

**State transitions** are mostly triggered by the **operating system**, via the **scheduler**.
### Scheduler
- Part of the OS **kernel**.
- Manages processor time and transitions between states.
- Handles **exceptions** and ensures fair execution of active processes.
## Deadlock Conditions
A **deadlock** is a condition where a set of processes become stuck indefinitely, waiting for each other. It occurs only when **all four conditions below hold simultaneously**:
1. **Mutual Exclusion** – Each resource is either free or exclusively held by one process.
2. **Hold and Wait** – A process holds resources while waiting for more.
3. **No Preemption** – Resources can't be forcibly taken; only voluntarily released.
4. **Circular Wait** – A cyclic chain of processes exists, where each is waiting for a resource held by the next.


# Message Passing

## Synchronization

### Types of Blocking Synchronization

1. **Rendezvous**
    - Both processes must reach a synchronization point before data exchange.
    - No intermediate storage required.
    - Common in **point-to-point** communication.
2. **Remote**
    - The sender blocks until it receives confirmation of message delivery.
    - May involve **intermediate storage**.
    - Typical in **shared communication channels**.

### Addressing Types
To exchange messages, sender and receiver must be identified.
- **Direct addressing**  
    The sender specifies the recipient explicitly.
- **Indirect addressing**  
    Communication occurs via a **channel or mailbox** rather than naming the recipient directly.  
    Some systems implement **mailboxes** to store and queue messages chronologically.

## Communication Types
- **One-to-One**  
    Message exchange between a **single sender** and a **single receiver**.
- **One-to-Many**  
    Message sent from one sender to **multiple recipients**:
    - **Broadcast** – Sent to _all_ processes.
    - **Multicast** – Sent to a _subset_ of processes.
- **Scatter**  
    The sender distributes different segments of a message to different receivers.  
    Each receiver gets a **distinct part** of the original data.
- **Gather**  
    Multiple senders contribute segments that are **merged into a single message** by the receiver.

# Introduction to Message Passage Interface (MPI)

## MPI: Message Passing Interface
MPI is a standardized library specification used for **parallel programming via message passing**, widely supported across C, C++, and Fortran.
### Error Handling
In MPI, **every function call** in C/C++ returns an **error code** that indicates the outcome of the operation. This design allows fine-grained error management and recovery mechanisms.
MPI allows error handlers to be associated with:
- **Communicators**
- **Windows**
- **Files**
If no specific association is made, the default assumption is that the function is linked to `MPI_COMM_WORLD`.
#### Predefined Error Handlers
- **`MPI_ERRORS_ARE_FATAL`** (default):  
    Aborts **all** processes when an error occurs — equivalent to calling `MPI_Abort`.
- **`MPI_ERRORS_RETURN`**:  
    Returns the error code to the user without halting execution, allowing the program to continue.

### Programming Language Data Types
MPI is **not a programming language**, but a **library specification**. It defines a set of **predefined data types** that correspond to native language types used in communication routines.
For example:
- `MPI_INT` maps to `int` in C/C++
- `MPI_FLOAT` maps to `float` in C/C++
This mapping ensures:
- **Type safety** in message exchange
- **Portability** across heterogeneous platforms

### Special MPI Data Types
MPI also includes **special data types** that do not directly correspond to typical programming language types:
- **`MPI_BYTE`**  
    Represents a raw 8-bit byte.  
    Useful for transferring **binary data** or performing low-level file I/O.  
    In C/C++, this is equivalent to using `unsigned char`.
- **`MPI_PACKED`**  
    Designed for **manual packing** of non-contiguous or heterogeneous data structures.  
    It’s essentially a **serialized byte array** that can hold multiple elements from different memory locations in a single message.  
    Perfect for custom data layouts or advanced memory management scenarios.

## Communicators in MPI
In MPI, a **communicator** defines an isolated **communication context**, or in other words, a private “universe” where processes interact. This ensures that:
- **Messages are only matched within the same communicator**.
- Different communicators prevent interference between unrelated communications.
### Key Properties
- A communicator encapsulates a **group of processes**.
- Each process in the group has a unique **rank** — an integer from `0` to `size - 1`.
- The group is **ordered and fixed** within the communicator.
### MPI_COMM_WORLD
- The most fundamental predefined communicator.
- Automatically available after `MPI_Init`.
- Includes **all processes** launched with the job.
- Most introductory MPI programs use this communicator exclusively.

## Process Identification and Size
Once `MPI_Init` is called:
- The **size** of the communicator can be obtained using `MPI_Comm_size`.
- The **rank** of the calling process (its ID in the group) is obtained using `MPI_Comm_rank`.
Ranks are **local to each communicator**. A process may have **different ranks** in different communicators.
These ranks are used for:
- **Point-to-point operations** (e.g., `MPI_Send`, `MPI_Recv`)
- **Collective operations** (e.g., `MPI_Bcast`, `MPI_Scatter`)

## MPI Messages
Each MPI message is composed of:
1. A **header** (also called the _envelope_)
2. An **optional payload** (the actual data)
### Header Fields
The header contains **metadata** used for message matching:
- **Communication Context**  
    Identifies the communicator in which the message exchange takes place.
- **Source Rank**  
    The rank of the sending process.
    - Implicit in send operations
    - Must be explicitly specified in receive operations
- **Destination Rank**  
    The rank of the receiving process.
    - Implicit in receive operations
    - Must be explicitly specified in send operations
- **Tag**  
    An integer used to **categorize messages** (e.g., command types, data types).
    - Valid range: `0` to `MPI_TAG_UB`
    - The upper bound `MPI_TAG_UB` is **implementation-defined** and can be queried via `MPI_Comm_get_attr`.
Using tags strategically allows a single sender/receiver pair to manage different message types independently.

### Information Content
Beyond the header (envelope), each MPI message includes a **payload** — the actual data being transmitted. This is described using three key parameters:
### Message Content Parameters
- **Data Type**  
    Specifies the type of each element in the message.  
    Example: `MPI_INT`, `MPI_FLOAT`, `MPI_DOUBLE`, etc.  
    Ensures **portable** and **type-safe** communication.
- **Buffer Reference**  
    A **pointer to the memory location** that contains the data to be sent (in `MPI_Send`)  
    or the destination where data will be received (in `MPI_Recv`).
- **Count**  
    Indicates the **number of elements** (of the specified type) to be transmitted.
    - Can be set to `0` for an empty message.
    - Even empty messages are **valid** in MPI and still carry header information (e.g., tag/context).

## Point-to-Point Communication (P2P) in MPI

**Point-to-point communication** is the most basic form of interaction in MPI, where **one process sends a message directly to another**.

This mechanism forms the foundation for building more complex communication patterns.

### Standard Communication Mode
MPI provides two core functions for P2P communication:
- `MPI_Send(source → destination)`
    - **Blocking send**: the function call doesn't return until the message is either **received** or **safely buffered**.
- `MPI_Recv(destination ← source)`
    - **Blocking receive**: the function call doesn't return until the **message has been received** and the **buffer is filled**.
These functions ensure **synchronous coordination** between the sender and the receiver under normal use.
The blocking behavior can lead to deadlock if not used carefully — especially when both sender and receiver are waiting on each other.

## Collective Communication 

**Collective communication** involves **multiple processes** cooperating within a defined **communication group**. Unlike point-to-point messaging, it provides **coordinated operations** that are **optimized** for performance and structure across distributed systems.

These operations are essential when data must be **shared, distributed, or gathered** across multiple processes in a scalable way.

### **MPI_Bcast** – Broadcast
- A **single message** from a **root process** is sent to **all other processes** in the group.
- Even the root receives the message (usually its own original data).
- Use case: **Distributing the same input/config to all processes** at once.

### **MPI_Scatter** – Scatter
- The **root process** sends **distinct chunks of data** to each process in the group.
- Each process receives a **unique portion** of a larger dataset.
- Use case: **Dividing work** so each process handles its **own data segment**.

### **MPI_Gather** – Gather
- All processes **send their local data** to the **root process**.
- The root collects and organizes it into a **single buffer or structure**.
- Use case: **Combining parallel results** into one location (e.g., for output or final processing).

### **MPI_Reduce** - Reduce
The operation performs an element-wise reduction across data from all processes in a communication group and delivers the result to a designated root process.
- Each process provides a **value array**.
- The values are **combined element-wise** using a **commutative binary operator** (e.g., `MPI_SUM`, `MPI_MAX`, `MPI_MIN`, `MPI_PROD`, `MPI_LAND`, etc.).
- The **root** process **receives the final result** of the **reduction**.
This operation is commonly used to collect and combine partial results computed in parallel.
- Use case: summing partial vectors across processes to obtain a global total.


## Non-Blocking Communication 

In contrast to standard (blocking) MPI operations, **non-blocking communication** allows a program to initiate communication and proceed without waiting for its completion.
### Key Functions:
- **`MPI_Isend`**  
    Initiates a non-blocking send. Returns immediately — the message may not yet have been transmitted.
- **`MPI_Irecv`**  
    Initiates a non-blocking receive. Returns immediately — the data may not yet have arrived.
These functions return **request handles**, which are later used to monitor or complete the communication.
### Completion Routines:
To ensure the communication completes correctly:
- **`MPI_Wait(request, status)`**  
    Blocks the caller until the specified request has completed.
- **`MPI_Test(request, flag, status)`**  
    Non-blocking check — returns immediately, indicating whether the communication has completed.
This approach enables **computation and communication overlap**, which is crucial for performance in large-scale parallel applications.

## Collective Synchronization
**Collective synchronization** is used to align the progress of all processes within a communicator. It ensures that no process proceeds past a certain point until all others reach it.
### MPI_Barrier(comm)
- Blocks **all processes** in the communicator until **every process** has reached the barrier.
- Once all are synchronized, they are released to continue execution.
**Use cases:**
- Measuring elapsed time accurately across processes.
- Enforcing consistency between computation phases.
- Coordinating access to shared or sequential resources.
All processes must call `MPI_Barrier` — failure to do so leads to deadlock.

## Sorting With MPI

Sorting is a fundamental operation in computing, especially critical for **large-scale datasets** — often in the range of **gigabytes or terabytes**. On such scales, **sequential sorting** quickly becomes a **performance bottleneck**, constrained by:
- The **speed** of a single CPU
- The **memory limitations** of one machine

### Why Parallel Sorting?
To overcome these constraints, sorting is parallelized using **MPI-based distributed algorithms**, enabling:
- Utilization of multiple processors or machines
- Reduction in total sorting time
- Scalability with growing data sizes

### General Parallel Sorting Workflow
A parallel sorting algorithm in MPI typically involves **three major stages**:
1. **Data Distribution**  
    The dataset is divided and distributed among the available processes.
2. **Local Sorting**  
    Each process independently sorts its assigned portion using a sequential sort.
3. **Global Merging or Redistribution**  
    The sorted sublists are then merged or redistributed to produce a **globally sorted result**.
This global phase often determines the **efficiency** and **scalability** of the algorithm.

### Algorithmic Approaches
Several parallel sorting algorithms are commonly used in MPI:
- **Parallel Merge Sort**  
    Local merges followed by global merging in a hierarchical or tree-based manner.
- **Sample Sort**  
    A scalable approach where samples are selected to determine splitters for balanced redistribution.
- **Bitonic Sort**  
    Often used in fixed-topology networks; relies on recursive merging of bitonic sequences.
- **Bucket Sort**  
    Distributes data into buckets (ranges) and sorts locally within each bucket.

Each method carries specific **trade-offs** depending on:
- Data characteristics (e.g., uniform vs. skewed distributions)
- Initial data layout
- Communication overhead
- Synchronization requirements

### Choosing the Right Algorithm
The selection of a sorting strategy should consider:
- **Dataset size and shape**
- **Target architecture** (e.g., cluster, NUMA system)
- **Desired scalability and performance goals**
Efficient MPI sorting is not just about computation — it’s about minimizing **communication** and **waiting time** across all processes.


## Parallel Merge Sort (MPI)

**Parallel Merge Sort** is a distributed sorting algorithm inspired by classical merge sort. It scales effectively across multiple MPI processes to handle large datasets that are impractical to sort sequentially.
#### 1. **Divide**
- The input dataset is partitioned evenly and distributed among MPI processes.
- Each process receives a chunk of the data to sort.
#### 2. **Local Sort**
- Each process applies a sequential sorting algorithm (like quicksort or mergesort) to its local chunk.
- The efficiency of this step significantly affects overall performance and the quality of the final merge.
#### 3. **Global Merge**
The challenge lies in efficiently combining the locally sorted chunks across all processes. Two main strategies are used:
- **Pairwise Merge:**
    - Processes form pairs and exchange data.
    - After merging, one retains the lower half and the other the upper half.
    - This process repeats for `log₂(P)` rounds (where `P` is the number of processes), progressively halving the range.
- **Merge Tree / Recursive Doubling:**
    - Processes are arranged in a logical binary tree.
    - Leaf nodes merge and send results to parent nodes.
    - This continues recursively until the root process has the globally sorted data.
    - This method is typically more communication-efficient.
#### 4. **Gather (Optional)**
- If a single process (e.g., rank 0) needs the full sorted result:
    - Use `MPI_Gather` or `MPI_Gatherv` to collect chunks.
- In many cases, keeping the data distributed is preferable, especially if further parallel computation follows.
![[image.png]]

# Slurm - Workload Manager














