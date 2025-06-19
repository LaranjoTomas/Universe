# 1. **HPC Definition and Evolution**: High-Performance Computing (HPC) has seen significant evolution. Describe what HPC encompasses today, beyond just hardware, including its core components. Furthermore, explain how the field has undergone a "paradigm shift" with the emergence of heterogeneous computing architectures. 

**R:** With the rise of heterogenous computing architectures such as those combining CPUs and GPUs, the field has seen a major paradigm shift, in GPUs with their inherent support for data parallelism. The Core components of HPC consist in hardware, software and Programming paradigms. Hardware from the multicore CPUs, GPUs and the clusters. The Software with the compilers and libraries being updated/created as time passes. And Programming Paradigms, which are techniques using modern technologies to better improve the efficiency of previous iterations.

# 2. **Role of GPUs in HPC**: Detail the **role of GPUs in modern HPC**, emphasizing their strengths in data parallelism. How do GPUs complement CPUs in a heterogeneous computing environment, and what impact has this had on programming approaches like CUDA?

**R:** GPUs are a great improvement over CPUs for tasks that excel at high need of parallel workloads, while CPUs are better for dynamic tasks that don't require parallel process. GPUs excel at data parallel tasks.

# 3. **Importance and Applications of HPC**: Beyond computational power, elaborate on **why HPC is important** by discussing the types of large-scale computational problems it solves and how it actively drives innovation across science, technology, and engineering disciplines. 

**R:** With the large-scale computational power of the HPCs of today, scientists, and other professions, are able to do mathematical formulas that would require months of time with humans alone. Just like computer science, weather forecasting, nuclear fission. It enables **innovation** in science, technology and engineering while driving **advancements** in hardware and software. 

# 4. **Parallel Decomposition Concepts**: Explain the concept of **parallel decomposition**, highlighting its typical data-driven nature. Discuss the three categories of granularity in parallel algorithms—fine-grained (SIMD), medium-grained (MIMD shared memory), and coarse-grained (MIMD distributed memory)—and how HPC algorithms often blend these levels for optimal performance. 

**R:** Parallel Decomposition is a technique used to speed up the execution of a program by splitting it into smaller, independent parts that can be executed in parallel. Granularity depends on the hardware and can be categorized into 3: fine-grained, medium grained, and coarse-grained. Fine-grained is when an operation can act on multiple data at the variable. Medium-grained is parallelism in the threads. Coarse-grained is between many processes.
# 5. **Amdahl's Law and Performance Limits**: Describe **Amdahl's Law** and its significance in estimating performance gains in parallel systems. How does the non-parallelizable fraction of a task limit overall speed up, and what additional factor further constrains speed up in distributed memory systems? 

**R:** Amdahl's law estimates the time/performance gain from parallelizing a section of the code and having it run in parallel with the rest of the code. The speed-up gain from having this in parallel would be the performance gain estimated by the amdahl's law. In the formula, the non-parallelized fraction, the higher it would be the less the overall speed-up would be in the end. The additional factor that further constrains speed up in distributed memory systems would be the latency or the overhead of the communications between the many processes.

$$ \frac{1}{(1-P) + (\frac{P}{X})} $$

P is the parallelized section. X would be ???
# 6. **Architecture of Modern HPC Systems**: Detail the architectural characteristics of modern HPC systems, specifically focusing on their primary operation as **distributed memory parallel architectures**. Explain how processing nodes are organized into extensive clusters and interconnected, and what challenges this architecture addresses. 

**R:** Modern HPC systems are interconnect via topologies which are designed to prioritize scalability, so that the system performance scaled with the addition of new nodes. The primary challenges this architectures address are to minimize the number of connections per node while the cluster grows, and ensure that the communication time and bandwidth remain constant.

# 7. **Instruction-Level Parallelism (ILP)**: Discuss **pipelining** as an implementation technique and how it contributes to Instruction-Level Parallelism (ILP). Elaborate on other key mechanisms that facilitate ILP, such as multiple-issue, branch prediction, speculative execution, out-of-order execution, and prefetching. 

**R:** Pipelining is when u subdivided a generic task in subtasks with the goal to make the generic task in a stream of subtasks in sequence, independent subtasks. Each subtask is executed in sequence and is a part of the overall generic task. ILP is facilitated by this mechanism that would enable parallel execution of instructions on each subtask. Multiple-issue is when multiple independent instructions can be initiated at the same time. Out-of-order execution is when, for example, in a program there are multiple lines of the code with different mathematical instructions (multiply, sum, ...) the order in which these are executed can be arranged to reduce the overall time it would take. Prefetching is when data is retrieved before an instruction would request it.

# 8. **Programs, Processes, and Threads**: Clearly differentiate between a **program, a process, and a thread**. Explain the properties that define a process and how the execution environment can treat resource ownership and the thread of execution separately. What are the key advantages of adopting a multithreading approach? 

**R:** While a program is a sequence of instructions that describe the execution of a task, the process would be the program executed. A process is made of a collection of resources and threads. A process has a private address space, I/O context, a state and its context in of itself. Resource ownership is a private address space dedicated for communication channels while a thread of execution is what a thread has for itself, like a program counter. Multithreading increases efficiency and execution speed, improves resource management by sharing the address space and I/O.

# 9. **Process and Thread States**: Describe the typical lifecycle **states of a process** (Running, Ready-to-run, Blocked, Created, Terminated) and the events that trigger transitions between these states. How does this compare to the state transitions of a thread, and who, or what, is responsible for managing these transitions?

**R:** The state transitions are triggered by external sources like the OS. The scheduler is the one responsible for managing the state transitions. The state transitions in both are similar but differ when the thread level is either user or Kernel. If it's kernel level blocking a thread does not block other threads so they can continue executing the instructions, while on user level blocking a thread will block the whole process.

# 10. **Process Interaction and Mutual Exclusion**: Analyze the different ways processes can interact in a multiprogrammed environment, distinguishing between **independent and cooperating processes**. Explain the concept of a "**critical region**" and the necessity of **mutual exclusion** to prevent race conditions and information loss. What are the negative consequences if mutual exclusion is not properly enforced or handled?

**R:** In concurrency, a critical region is the section of a code where when executed it will access a shared resource or address space. The issuefrom this comes when 2 or more processes are in the critical region of different tasks at the same time, they will try to get the shared resource at the same time and alter it if needed. This will lead to many problems due to only 1 process being able to alter a shared resource at the same time.

# 11. **Deadlock Conditions and Prevention Strategies**: Enumerate the **four necessary conditions for deadlock** to occur. Choose *two* of these conditions (excluding mutual exclusion) and for each, describe a specific strategy that can be implemented to deny that condition, thereby preventing deadlock. Discuss any potential drawbacks or challenges associated with these prevention strategies.
Circular wait, no preemption, mutual exclusion, hold and wait
**R:** For deadlock to occur, 4 conditions must exist at the same time, these are:
- Circular wait - 
- No Preemption
- Mutual exclusion
- Hold and Wait



8. **Message Passing Synchronization**: Explain the general principle of **message exchange** as a communication method in parallel computing. Compare and contrast "**blocking synchronization**" and "**non-blocking synchronization**" in message passing, describing how each type operates for both sending and receiving messages and their implications for program flow and complexity. 
9. **MPI Communication Models**: Differentiate between **point-to-point communication** and **collective communication** in the context of MPI. For point-to-point, describe the `MPI_Send` and `MPI_Recv` operations. For collective communication, select and explain *two* specific operations (e.g., `MPI_Bcast`, `MPI_Scatter`, `MPI_Gather`, `MPI_Reduce`), including their conceptual model and typical use cases in parallel programming.