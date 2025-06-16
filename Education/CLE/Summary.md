---
tags:
  - Summary
  - CLE
---
# What is High-Performance Computing (HPC)
- Use of powerful resources to solve complex problems
	- Multicore CPUs, GPUs, clusters
	- Goals: high throughput, efficiency
- Importance in fields like AI, weather, and physics
- Emerging paradigms: heterogeneous computing
# Parallelism Models and Architectures
- Coarse-grain: tasks run independently (e.g., MPI)
- Medium-grain: shared memory threads (e.g., std::thread)
- Fine-grain: per-instruction (e.g., CUDA)
## Architectures 
- Shared vs. distributed memory
- Interconnection topologies: mesh, torus, tree, fat-tree

# Concurrency and Synchronization

- Independent vs. Cooperating processes
- Mutual exclusion and critical regions
- Deadlock, livelock, starvation

# Resource Management and Deadlock Conditions

- Preemptable vs. Non-preemptable resources
- Four deadlock conditions: mutual exclusion, hold and wait, no preemption, circular wait

# Thread-Level Parallelism

**Thread Pools will not be on the test**
# Message-Passing Programming
- Communication without shared memory
- Blocking vs. Non-blocking synchronization
- Direct and indirect addressing (mailboxes, ports)
## Communication Patterns
- One-to-one, broadcast, multicast
- Scatter and gather
- Producer-consumer models with mailboxes

# MPI: Message Passing Interface
- Standard API for inter-process communication
- MPI_Init, MPI_Finalize, MPI_Comm_rank, MPI_Comm_size
- Compilation and execution using mpic++, mpiexec

- Error handlers: MPI_ERRORS_ARE_FATAL, MPI_ERRORS_RETURN
- Communicators define communication contexts (e.g.,MPI_COMM_WORLD)

## Collectiove Communication
- Broadcast, Scatter, Gather, and their signatures
- Blocking nature and use cases

### Non-Blocking Communication
- MPI_Isend, MPI_Irecv, MPI_Wait, MPI_Test
- Overlap computation and communication
- Use cases and performance considerations

# SLURM Workload Manager

- Open-source job scheduler for Linux clusters
- Used in top supercomputers (El Capitan, Frontier)

- Components: slurmctld, slurmd, slurmdbd, slurmrestd
- Job submission: sbatch, srun
- Configuration: slurm.conf, partitions, node definitions

# CUDA Programming
- CUDA model: kernels, thread blocks, memory hierarchy
- **global** functions launched with <<<grid, block>>>
- Memory management: host vs. device memory

## Execution and Scalability
- Thread/block/grid hierarchy
- Scalability across GPU architectures
- Compilation with nvcc

