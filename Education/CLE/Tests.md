
# Test 2

## 1. Parallel decomposition of a problem is typically data-driven. Chunks of data of the input stream are fed to a T-stage pipeline of operations. At each stage, data is further split so that operations may be carried out independently in mutual exclusive parts of the chunk being processed. In between stages, the data
chunks may undergo reshuffling.

![[image-2.png]]

### a)Parallel algorithms are usually designed with various degrees of granularity which depends on the hardware platform where the code is supposed to be run. What is granularity? (1 point)


### b)What are the main categories parallelism may be divided in? Explain how they fit in a distributed- memory parallel machine. Justify your claims carefully. (2 points)

### c) Performance gains to be achieved when an application is run in a parallel machine are limited by the Law of Amdahl. State the Law of Amdahl and apply it when the potentially enhancing fraction is 0,8 and the speed up of the enhanced fraction 4. Justify your claims carefully. (2 points)




# Test 1 ig

## 1.

![[image-3.png]]

### a) 
**R:** Distributed-memory parallel machines are HPC systems that are organized into clusters of processing nodes interconnected via topologies, with the configuration prioritizing scalability and improving the communication overhead within the nodes. Each node has its own private memory and does not share memory directly, it needs to communicate by passing messages to exchange data. Distributed-memory parallel machines contrasts with shared-memory systems where all processors access a common memory space.

### b)
**R:** 
- **CPUs -** These are general purpose cores ideal for decision-making and control flow
- **GPUs -** Many-cored components optimized for massive parallel processing. Used for data-parallel tasks
- **Local memory -** Each node has its own memory, directly accessible only by the processors in that node. Stores data and instructions
- **Communication controller -** Manages communication between nodes across the network. Responsible for sending and receiving messages.

### c)
**R:** **Communication overhead** refers to the **time and resource cost** associated with data exchange between nodes. 
Reduce message sizes and frequency, use topology-aware scheduling, optimized communication patterns.

## 2. 

![[image-4.png]]

### a)
**R:** In my most humble opinion, multithreading would be the ideal alternative within a single process. 
- **Lower overhead:** Threads share the same memory space, making communication between them faster and more efficient
- **Easier data sharing:** Threads can access shared variables directly
- **Better performance on multicore systems:** Threads are light-weight processes and therefore more efficient than them

### b)
**R:** The racing conditions that need to be avoided in concurrent programs are Deadlock, Postponement. Postponement occurs when one or more processes compete for access to a critical region, but because of new competing processes, access is repeatedly denied. Deadlock occurs when two or more process remain indefinitely waiting for access to their respective critical regions.

## 3.

![[image-5.png]]

### a) 
**R:** It is organized in envelop/Header and content. A message in MPI is a structured unit of data used for communication between processes. 
A message typically includes:
- Data buffer,
- Data type
- Communicator
- Source and destination rank

### b)
- **Scatter:** A root process divides a dataset and distributes parts of it to all other processes including the root.
- **Reduce:** Each process performs a local computation on its part of the data and then the results of all processes are combined and returned to the root

![[image-6.png]]

### c)
**R:** A barrier is used when it's needed to synchronize all the processes in a device. When implementing a barrier function in a program when a process reaches that part of the code it will stop and wait until the last process active reaches the barrier. When the last process reaches the barrier it will notify all the other processes waiting, and then they can continue executing the program.

## 4.

![[image-7.png]]
### a)
**R:** CUDA divides its operations by the host and the device in 5 distinct main steps.
- First, memory allocation in the GPU 
- Second, Data transfer from the memory of the CPU to the memory allocated in the GPU
- Third, The kernel is launched and the cuda computation starts with the program the user specified
- Fourth, The result is then transferred back to the CPU memory from the GPU
- Fifth, The allocated memory is destroyed/freed from the GPU

### b)
**R:** When launching a kernel in CUDA, you must specify its **computation Geometry**, which defines how the work is divided across the GPU cores. This is divided in Grid and block which both can be 1D, 2D or 3D. 
With the correct geometry there are obvious benefits to the code execution. The kernel will scale across different GPU architectures when all have the same geometry; more threads allow higher degrees of parallelism, too few threads underutilizes the GPU too many can cause overflow 