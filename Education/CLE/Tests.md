
# Test 2

## 1. Parallel decomposition of a problem is typically data-driven. Chunks of data of the input stream are fed to a T-stage pipeline of operations. At each stage, data is further split so that operations may be carried out independently in mutual exclusive parts of the chunk being processed. In between stages, the data chunks may undergo reshuffling.

![[image-2.png]]

### a)Parallel algorithms are usually designed with various degrees of granularity, which depends on the hardware platform where the code is supposed to be run. What is granularity? (1 point)

**R:** Granularity refers to the amount of computation performed between two communication or synchronization events in a parallel algorithm.
### b)What are the main categories parallelism may be divided in? Explain how they fit in a distributed- memory parallel machine. Justify your claims carefully. (2 points)

**R:** There are 3 main categories that parallelism may be divided in: Fine-grained, Medium-grained and Coarse-grained.
- **Fine-Grained** is SIMD where operations happen on multiple data at the variable level.
- **Medium-Grained** is MIMD (shared memory), parallelism at the thread level
- **Coarse-grained** is MIMD, parallelism at the process level.
### c) Performance gains to be achieved when an application is run in a parallel machine are limited by the Law of Amdahl. State the Law of Amdahl and apply it when the potentially enhancing fraction is 0,8 and the speed up of the enhanced fraction 4. Justify your claims carefully. (2 points)

**R:** $$ \frac{1}{(1-P) + (\frac{P}{X})} $$
The Law of Amdahl estimates the speed up or performance gain when a section of the program is parallelized compared to the normal execution time when the program isn't.

$$ S = \frac{1}{(1-0.8) + (\frac{0.8}{4})} = 2.5 $$

## 2. Concurrency, as a means of designing the local part of a parallel application to be run in a processing node, has become very popular.

### a) What is concurrency? How may it enhance program execution? (1.5 points)

**R:** Concurrency is the ability of a program to make progress on multiple tasks simultaneously. It can improve resource utilization, faster execution.
### b) Synchronization is usually required, but introducing too much synchronization may degrade the execution performance. Why is it so? Present your claims carefully. (1.5 points)

**R:** Synchronization will ensure that threads or processes access shared resources safely and in a consistent order, preventing race conditions and data corruption from happening. However, too much synchronization can degrade the performance due to:
- Blocking and waiting, when a thread enters a synchronized section other threads must wait even if they are ready to run this of course will reduce efficiency in a concurrency program
- Too much synchronization may increase the complexity of the code which can raise the changes of deadlocks happening
- System calls to memory barriers (wait, test) may increase overhead
### c) Furthermore, two pathological conditions may come about: deadlock/livelock and indefinite postponement. What are they? Do they affect program execution in different manners or in the same manner? (2 points)

**R:** Postponement occurs when one or more processes compete for access to a critical region, but because of new competing processes, access is repeatedly denied. Deadlock occurs when two or more process remain indefinitely waiting for access to their respective critical regions.

## 3. MPI (Message Passing Interface) is used to write parallel programs to be run in several nodes of a distributed-memory parallel machine.
### a) A basic concept in MPI is a group of processes that will cooperate in running an application. How is the group usually instantiated? (1 point)

**R:** In MPI, the group of processes that cooperate to run a parallel application is usually instantiated through a communicator, the default one being **MPI_COMM_WORLD**. 

### b) One of the communication paradigms that can be used is the scatter-gather paradigm. Explain how it works. Make a sketch of its operation using four processes. (2 points)

**R:** 
![[image-9.png]]

In the communication paradigms, context scatter consists in the root dividing a dataset and then sending different sections of this dataset to each process including itself the root. After this in Gather, all processes will send their local data to the root process, the root collects all the results and organizes it into a single structure.
### c) Draw an interaction diagram where 5 processes cooperate into computing the product of two order 5 square matrices. The operand matrices are stored in a file, and the result matrix should also be stored in a file. Describe the role you have assigned to each process and explain how they are synchronized. (2 points)

**R:** 
![[image-10.png]]


## 4. CUDA is used to program the GPU (Graphics Processing Unit) devices.

### a) What is CUDA C? (1.5 points)

**R:** CUDA C is an extension of the C programming language developed by NVIDIA to allow programmers to write programs that run on the NVIDIA GPUs. Especially, CUDA C adds keywords and constructs to standard C to define functions called kernels that run on the GPU. 
### b) A typical program written in CUDA divides its operations by the host and the device in several main steps. Which are usually the steps carried out by the host and the steps carried out by the device? (1 point)

**R:** CUDA divides its operations by the host and the device into 5 main steps. These steps are:
- Allocating memory in the GPU
- Transferring/Copying the Data from the CPU to the GPU
- Invoking the kernel on the GPU to compute the program specified by the user
- Transferring/Copying the data from the GPU back to the CPU
- Destroying/Freeing the memory allocated in the GPU

Host:
- Executes all of the above
Device:
- Only executes the kernel

### c) Why is the access to operands located in the local memory, or in the shared memory, faster than the access to operands located in the global memory? Justify your claims carefully. (2 points)

**R:** The local memory is a private memory for each thread of the GPU, while the shared memory is shared between all the threads in a single block. The global memory is shared between every single thread in the GPU. The faster access to the local/shared memory is due to the location of this memories being much closer to the components that use them/need them.
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
With the correct geometry there are obvious benefits to the code execution. The kernel will scale across different GPU architectures when all have the same geometry; more threads allow higher degrees of parallelism, too few threads underutilizes the GPU too many can cause overflow, having a balance will allow for greater efficiency and resource usage.

### c)
**R:** In cuda there are 5 types of memory, each specialized for different usage patterns. Local memory is private for each thread, shared memory is shared between blocks of threads and global memory is shared for all the threads in the GPU. The proximity to the cores (process or threads) that use them will of course lead to faster access, since the global memory would be in a location more distant than shared or local memory.


# Test 2021

## 1. Present day high performance computers are at the top level distributed memory parallel machines. They may be thought of as vast clusters of processing nodes (PNs) interconnected by some network topology.

### a) The reason for the fact is scalability. What is scalability? (1 point)
**R:** Scalability refers to the ability of a parallel computing system or algorithm to effectively handle increasing amounts of work or to efficiently utilize and increasing number of components that may be added at any time without a significant drop in the overall performance.
### b) The interconnection topology is paramount in what concerns scalability. What are the main properties one expects of the interconnection topology? Give two examples of interconnection topologies being used today and explain how they behave in connection to the properties you mentioned. (2 points)
**R:** Scalability ofc and not increasing the communication overhead between the nodes in the topology counting the ones that may be added. 
Fat Tree topology is a hierarchical tree-like network where the higher level links have more bandwidth to handle aggregated traffic from lower levels.
Star topology, all processing nodes are connected directly to a central switch or hub which acts as a communication manager.

### c) Sketch the main components of a typical processing node. What does one mean by heterogeneous computing in this context? Justify your claims carefully. (2 points)

**R:** Heterogeneous computing refers to the evolution of the parallel programming where it evolved to use both CPUs and GPUs together. 
Typically this means combining CPUs that are optimized for sequential and control taks with GPUs which is optimized for parallel tasks.
![[image-11.png]]

## 2. Multithreading, as a means of writing concurrent applications that run in a processing node, has become very popular.

### a) Explain what multithreading is and why it is so. Justify your claims carefully. (2 points)

**R:** Multithreading is an efficient mechanism that allows a single process to have more than 1 thread running at the same time with shared address spaces. Each of these threads can execute different instructions, Multithreading is a more efficient than certain other mechanisms in concurrency, is simple to use and shared an addressing space which makes it easier to manage data.
### b) When writing a concurrent application, two pathological conditions may come about: deadlock/livelock and indefinite postponement. What are they? How do you deal with them so that the program may run correctly? (2 points)

**R:** Indefinite Postponement occurs when one or more processes compete for access to a critical region, but because of new competing processes, access is repeatedly denied. Deadlock occurs when two or more process remain indefinitely waiting for access to their respective critical regions.


## 3. MPI (Message Passing Interface) has been used to create parallel applications to be run in several nodes of a distributed-memory machine.

### a) However, for the application to run efficiently and to take full advantage of the processing power that is available, one must minimize interprocess communication. Why is it so? (1 point)
**R:** Communication between processes is expensive compared to computation. In MPI, processes communications may introduce latency even with high-speed interconnections, communication will always be slower than local memory access or computation. Delays may also be added due to synchronization that comes from some kinds of communications. With the increase of scale of the systems, multiple processes may need to communicate at the same time which can overload the network.
### b) One of the communication paradigms that can be used is the scatter-gather paradigm. Explain how it works. Make a sketch of its operation using four processes. (2 points)

**R:** 
![[image-9.png]]

In the communication paradigms, context scatter consists in the root dividing a dataset and then sending different sections of this dataset to each process including itself the root. After this in Gather, all processes will send their local data to the root process, the root collects all the results and organizes it into a single structure.

### c) Sometimes, part of the processes that cooperate must be synchronized as a group. A barrier synchronization device is then used. Explain how it works. (2 points)

**R:** A barrier synchronization is a function for collective synchronization used in MPI that does the following. When added to a program when a process reaches the section of the code with the function MPI_Barrier it will stop in its tracks there, this will repeat for all other processes that reach this point as well. The last process when it reaches the barrier will release all the other processes that were waiting in the barrier allowing for everyone to continue.

## 4. CUDA has been used to program the GPU (Graphics Processing Unit) devices.
![[image-12.png]]

### a) 
**R:** To take advantage of a **GPU** in a processing node, a program must be organized to **offload** the computationally intensive, data-parallel tasks to the GPU while letting the control and sequential tasks be handled by the CPU. It must identify parallelizable tasks, divide the work between the CPU and GPU, allocate memory to the GPU, transfer data, launch the kernels, transfer results back to host and then free GPU resources.

### b) Programming a GPU entails enforcing fine-grained parallelism where a specific computation kernel is run in several streaming multiprocessors, or SIMD (Single Instruction - Multiple Data) processors. Sketch the internal organization of such a processor and explain what is the meaning of a WARP in this context. (2 points)

**R:** A warp is a group of 32 threads that are executed by a single **SM** in a GPU. In this context, a warp represents how fine-grained parallelism is enforced in the GPU.

![[image-13.png]]

### c) When writing a computation Kernel, one should be aware of two optimization factors to minimize execution time. Which are they? Justify your claims carefully.

**R:** Two optimization factors to minimize execution time when writing a computation Kernel would be: Memory access optimization and Thread Configuration and utilization. These two factors would be the most crucial to minimize execution time, altering the size of the grid and block of the configuration of the kernel would also lead to other benefits like scalability, memory access optimization would minimize memory bandwidth bottlenecks.