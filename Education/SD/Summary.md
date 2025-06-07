---
tags:
  - "#Summary"
  - "#SD"
---
# Key Learning outcomes

- Grasp design issues in distributed Systems,
- Tackle concurrency and consistency in real Systems,
- Apply Java RMi and message passing, java is no more than a tool to implement/use distributed systems

# Concurrency

- Computer architecture
	- CPU
	- Main Memory
	- Pipelining
- Program vs Process
	- Process State
- Multithreading
	- Organization
# Architectual Models

- Client-To-Server: Centralized resource control
- Peer-to-Peer: equal roles, replicaton
- Publisher: ....

# Fundamental Models
- **Interaction:** bandwidth, latency, jitter
- **Failure:** omission, timing, byzantine
- **Security:** process and channel threats, access control
**Latency** is the time it takes to get the something from one device to the other, **Jitter** is the variation in that time delay.  

# Communication Fundamentals

- **Latency, transfer rate, bandwidth**
- **Synchronous** vs **asynchronous** primitives
- **Blocking** vs **non-blocking** operations

# Middleware and Sockets

- **TCP**: reliable, bidirectional, ....
- **UDP**
- **Socket** identified by Ip and port


# Remote Invocation Principles
- Marshaling/unmarshaling (serializing and unserializing) of data
- Stub acts as proxy for remote objects
- Communication via structured messages

# Client-Server Architecture

- Server base thread + proxy agent
- Mixed architecture: server also acts as client
- Java serialization simplifies implementation

# General Principles
- **independent** vs **cooperating** processes
- **Critical regions:** mutual exclusion essential
- **Deadlock** and **indefinite postponement**


# synchronization Devices

- **Monitors**: encapsulated access with wait/signal
- **Semaphores:** general-purpose mutual exclusion
- Java concurrency tools: barries, locks, atomic ops

#  Synchronization Techniques

- Cristian's Method (UTC Server)
- Berkeley Algorithm (internal synchronization)
- NTP: hierarchical, resilient

# Lamport Clocks
- Capture **happened-before** relation
- Scalar timestamps ensure partial ordering
- Extended timestamps for total ordering