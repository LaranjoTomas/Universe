
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

## Cloud Computing

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
![[e-mail_model.png]]

## Fundamental Models in Distributed Systems

In distributed systems, where processes cooperate and communicate over a network, **performance** depends heavily on the efficiency of message exchange. When remote services are involved, response times rely not only on server load and performance but also on network routing, transfer capabilities, and delays in software components. 
To achieve fast interactive responses, systems should have:
- Few software layers
- Small amounts of data transferred per interaction

However, **performance** is not the only concern; other quality attributes include:
- **Reliability**
- **Security**
- **Adaptability** to changing configurations and resource availability

For time-critical applications, ensuring necessary computing and network resources are available at the right times is essential.

### Performance of a Communication Channel
Communication channels may be realized by continuous data streams or by message passing. Key properties include:
- **Latency:**  
    The delay from when the sender starts transmitting to when the receiver starts receiving. It depends on OS processing times, network access delays, and routing overhead.
- **Bandwidth:**  
    The total amount of data that can be transmitted over the network in a given time.
- **Jitter:**  
    The variation in time to deliver a sequence of similar messages between endpoints.

### Variants of the Interaction Model

Distributed systems vary in timing guarantees, spanning two extremes:
- **Synchronous Distributed Systems:**
    - Each process step’s execution time has known lower and upper bounds.
    - Message delivery has a known upper bound.
    - Local clocks have a bounded drift from real time.
- **Asynchronous Distributed Systems:**
    - Each process step’s execution time has an arbitrary (but finite) upper bound.
    - Message delivery has an arbitrary (but finite) upper bound.
    - Local clocks may drift arbitrarily from real time. 

### Failure Model
In distributed systems, processes and communication channels can fail, deviating from their intended behavior. Failure types include:
- **Omission Failures:**  
    Prescribed actions do not occur.
- **Timing Failures:**  
    Actions occur but do not meet timing constraints.
- **Arbitrary (Byzantine) Failures:**  
    Unexpected errors occur temporarily or permanently due to malfunction in any component.

### Failure classification

![[Failure_classification_sd.png]]

### Security Model

Security in distributed systems is ensured by protecting processes, communication channels, and resources from unauthorized access.
- Resources may have different access rights, supporting privacy and sharing across various user classes.
- An **authority (principal)**—a user or process—is associated with each operation invocation and result.
- The **server** verifies the principal's identity and access rights before performing operations or rejects requests otherwise.
- The **client** verifies the server’s identity to ensure replies come from the intended server.

An **enemy** (adversary) is assumed capable of:
- Sending arbitrary messages to any process.
- Reading any message exchanged between processes.
The adversary might connect legitimately or unauthorized to the network.
Threats include:
- **Threats to Processes:**  
    Processes may receive messages whose sender identity they cannot verify.
- **Threats to Communication Channels:**  
    The adversary may copy, alter, or inject messages in transit, threatening privacy, integrity, and availability.
    

# Message Passing
## Communication systems

### Performance
- **Key Performance Factors**:
  - **Latency**: The delay between sending a message and the start of reception (equivalent to sending an empty message)
  - **Data Transfer Rate**: The speed at which data is transmitted between sender and receiver
  - **Bandwidth**: System throughput, measured as the volume of message traffic processed per unit of time
  - **Total Transmission Time** = $$ Latency + \frac{Message Length}{Data Transfer Rate} $$
- **Quality of Service (QoS)**:
  - Defines the system's ability to meet **deadline constraints** for **transmitting and processing** **continuous data flows***
  - Requirements: **Latency** must stay **below** a defined **upper limit**, and **bandwidth** must stay **above** a defined **lower limit**
  
- **Reliability and Error Handling**:
  - Modern **communication systems** are **highly reliable**
  - **Failures** are more often caused by **software errors** than network issues
  - Error detection and correction is delegated to applications (following the end-to-end argument)

### Communication Abstraction
- **Communication systems** should be **integrated and abstract**, **hiding** the complexity of **underlying physical networks**
- **Network software** is organized in a **hierarchy of layers** for a structured approach
- Each layer presents an **interface** to the **layer above**, describing the communication system logically
- Data encapsulation occurs at each layer:
  - **Sending side:** Each layer receives data from above, encapsulates it, and passes it down
  - **Receiving side:** Data is processed in reverse, with each layer removing encapsulation and passing it upward
## Programming Interface
### Middleware and Sockets
#### Role of Middleware
- Middleware provides an **abstraction layer** for communication between processes that do **not share an address space**.
- It introduces a communication device known as a **socket**, which serves as an endpoint for inter-process communication.
![[MiddleWare and sockets.png]]
#### Socket Characteristics

A **socket** is uniquely identified by:
- **IP Address** – Identifies the host computer system.
- **Port Number** – Specifies the endpoint of a communication channel within that system.

## Transmission Control Protocol (TCP)

- **Connection-Oriented:** A virtual communication channel must be established before any data exchange.
- **Bidirectional:** Once connected, data flows in both directions between endpoints.
- **Asymmetric:** Designed for the **client-server model**, where each endpoint has a distinct role.
### TCP Protocol
- **Client Side**:
  1. Instantiate Communication Socket
  2. Connect to Server (using server's public address)
  3. Open Input & Output Streams
  4. Write Request to the server
  5. Read Reply from the server
  6. Close Streams & Communication Socket

- **Server Side**:
  1. Instantiate Listening Socket (binds to server's public address)
  2. Continuously Listen for client connection requests
 
  3. When a request arrives:
     - Instantiate Communication Socket for client
     - Create and Start a Service Proxy Agent

  4. Within Service Proxy Agent:
     - Open Input & Output Streams
     - Read Request from Client
     - Execute Local Processing
     - Write Reply to Client
     - Close Streams & Communication Socket
## User Datagram Protocol (UDP)

- **Connectionless:** No need to establish a communication channel before sending data.
- **Unidirectional:** Designed to send a single message from one endpoint to another.
- **Symmetric:** No predefined roles; both endpoints act equivalently.
### UDP Protocol

- **Socket Types**:
  - Unlike TCP, UDP does not require a connection before data exchange
  - Messages (datagram packets) are sent directly from sender to receiver
  - **Receiving Socket**: Instantiated by the receiver at a specific port, listens for incoming packets from multiple sources
  - **Sending Socket**: Instantiated by the sender to transmit packets, can send messages to multiple destination addresses

- **Communication Flow**:
  - **Source Side**:
    1. Instantiate Send Socket
    2. Convert Message to Byte Array
    3. Instantiate Data Packet (containing the byte array and destination address)
    4. Send Data Packet to the receiver
  - **Destination Side**:
    1. Instantiate Receive Socket (binds to a public address/port)
    2. Receive Data Packet from the network
    3. Convert Byte Array to Message
    4. Process the Received Message

## Transformation Principles

- **Changes for Distributed Execution**:
  - **Minimal modifications** should be applied to the **interaction mechanism** among entities
  - **Cooperating processes and shared resources** reside in **different computer systems (no shared address space)**
### Implications for Remote Method Invocation:
- **Method invocation** must occur via explicit message exchange:
  - A **request message** is sent for method invocation
  - A **response message** is sent back with the return value
- **Message Content**
  - Must include **method parameters and return values**
  - Must carry **caller process attributes** relevant to execution
- **Pass-by-Value Requirement**
  - All parameters must be **passed by value** since **direct memory sharing is not possible** 

### Message representation and Interpretation
- Messages are transmitted through **communication channels**.
- At the **lowest level**, a message is simply an **array of bytes**.
- Since the client and server are **separate programs**, the receiver must be able to **correctly interpret** the incoming byte array.

Ensuring Proper Message Interpretation. A well-formed message must include:
- **Parameter values**
- **Parameter types**
- **Data structure information**

### Marshaling and Unmarshaling
#### Marshaling
- The process of **converting parameters and data** into a structured message for transmission over the network.
#### Unmarshaling
- The reverse process: **extracting parameter values** from a received byte array for interpretation by the receiver.

Java **automatically handles** marshaling and unmarshaling, developers only need to define the message types as implementing the **Serializable** interface.

## Client Architecture

### Changes in the Main Thread Execution

- **Previously**:  
    The main thread was responsible for instantiating:
    - Cooperating processes
    - Shared resource (locally)
- **Now**:
    - The **shared resource resides remotely** in a different address space.
    - It **cannot be instantiated locally**.

### Remote Reference (Stub)
- A **remote reference (stub)** is instantiated in place of the actual shared resource.
#### Stub Instantiation Parameters:
- Server’s **Internet address**
- Server’s **listening port**
- Any **other initialization values** must be sent using a **method invocation** on the stub.
#### Responsibilities of the Stub:
- Intercept method calls on the shared resource.
- Convert them into **message exchanges**.
- Send **requests** to the server.
- Receive and return **responses** to the caller.
### Stub for the Shared Resource

#### Structure:
Acts as a **proxy for remote method invocation**.
##### Stub Operation Steps (Per Method Call):
1. **Open Communication Channel**  
    (e.g., TCP socket connection to the server)
2. **Create Outgoing Message**  
    Contains:
    - Method identification
    - Parameters
    - Caller process attributes
3. **Send Message** to the server
4. **Receive and Validate Response**
5. **Update Caller State**  
    (based on the result)
6. **Close Communication Channel**
7. **Return Method Result**
### Data Type Changes
In the **Main Thread**:
- Replace instantiation of the actual shared resource with its **stub**.
- **Initialization values** are sent via a method call on the stub.
- Add logic to call `shutdown()` on the stub if the server must be shut down.
In the **Cooperating Processes**:
- Pass the **stub reference** instead of the shared resource reference.
### Data Type for Communication Channel
Encapsulates all **socket operations**:
- Connection setup
- Message transmission
- Message reception
- Connection termination
### Data Type for Messages
#### Design Options:
1. **Single Message Type**  
    One data structure that handles all message scenarios.
2. **Multiple Message Types**  
    Separate structures for:
    - Requests
    - Replies
    - Errors
    - Specific method calls

## Server Architecture

### Base Thread (Main Server Thread)
- The **shared resource** is passive and **instantiated by the base thread**.
- A **communication channel (socket)** is opened to listen on a **public address**.
- Upon receiving a request:
    1. A **Service Proxy Agent Thread** is created.
    2. The base thread immediately resumes listening.
    3. This model supports **server replication**, enabling concurrent request handling.

### Service Proxy Agent (Per-Request Handler)
- Receives and **decodes the incoming message**.
- **Extracts process attributes** from the message.
- Becomes a **client clone** by initializing those attributes.
- **Invokes the method** on the shared resource.
- Builds and sends a **response message** back to the client.
- **Closes the communication channel** and self-terminates.

### Key Data Types
#### 1. Main Thread (Server Base Thread)
- **New type** but largely reusable across servers.
- Required Customizations:
    - The **public address** for service exposure.
    - Specific **shared resource instantiation**.
    - Server-specific **resource interface instantiation**.

#### Service Proxy Agent Thread
- **New type**, generally reusable.
- Must:
    - **Implement interfaces** of all possible client classes.
    - Provide methods to **set/get client attributes**.

#### Interface to the Shared Resource
- **New**, invariant across implementations.
- Public method:
    - `processAndReply()` – performs the entire service lifecycle:
        1. **Validate incoming message**
        2. **Decode and extract client attributes**
        3. **Invoke resource logic**
        4. **Construct and return response message**

#### Shared Resource
- Mostly unchanged.
- Modification:
    - Replace references to cooperating processes with **service proxy agent references**.

#### Communication Channel
- **New type**, used by both server and proxy agents.
- Responsibilities:
    - **Socket creation and handling**
    - **Sending/receiving messages**
    - **Managing the connection lifecycle**

#### Message Data Type
- **Shared between client and server**
- Supports:
    - **Serialization / marshaling**
    - **Parameter encoding**
    - **Reply formatting and decoding**

## Mixed Architecture

### Key Concept
- In complex **distributed systems**, multiple **servers** are involved.
- A **server** may:
    - Provide services to clients.
    - **Request services** from other servers.
- Thus, a single server often **acts both as a client and a server**.

### Implementation Approach
- The dual role introduces **no conceptual complexity**:
    - The server **accepts requests** like any other server.
    - It also **sends requests** like a client.
- This leads to a **mixed architecture**, where:
    - Each server includes both **client and server modules**.
    - The **server role** handles **incoming service requests**.
    - The **client role** initiates **outgoing requests** to other services.

#### Structural Adjustments
##### 1. Dual Instantiation
Each Server must instantiate:
- A **listening socket:** for receiving client or peer server requests
- A **communication module:** for sending requests to other servers.
##### 2. Unified Message Format
- Message Structures remain **consistent** across roles
- Supports seamless marshaling/unmarshaling and interpretation
##### 3. Concurrency Management
- Essential to handle:
	- Multiple **incoming connections** (server side)
	- Multiple **outgoing connections** (client side)
- Proper synchronization and thread management are critical for:
	- **Responsiveness**
	- **Scalability**
	- **Reliability**

# Remote objects

## Remote Procedure Call (RPC)

### Addressing Space Separation
In an **RPC**, the **calling process (client)** and the **shared resource (server)** operate in **different address spaces**, often on **separate machines**.

This separation introduces several **key requirements** on both the client and server sides:

![[RPC_SD.png]]
#### Addressing Space A (Client Side)
- A **reference to the shared resource** must be:
    - **Obtained before any method invocation**.
    - Represented as a **stub (or proxy)** that:
        - Encapsulates communication logic.
        - Behaves like the real object but routes calls remotely.
##### Stub Configuration
- The **stub** must be configured with:
    - The **server’s public IP address**.
    - The **listening port**.
- Enables the client to perform **remote invocations** seamlessly.
#### Addressing Space B (Server Side)
- A reference to the shared resource must be:
    - **Created and instantiated** on the server.
    - **Made discoverable** to clients, either by:
        - **Binding** it to a known public address and port.
        - **Registering** it with a **naming or directory service**.

### **Key Implication:**
Since there is **no shared memory** between the client and server, **all interactions** must occur via **explicit message exchanges**.

### Remote vs Local Procedure Calls

While **Remote Procedure Calls (RPCs)** are designed to **mimic local procedure calls**, several fundamental differences must be considered when developing **Distributed Systems**

#### 1. **Possibility of Failure**

Unlike normal procedure calls, **remote calls can fail** for reasons **unrelated to the application logic**, such as:
- The **shared resource is not instantiated** or temporarily **unavailable**.
- **Network Issues**
**Reliability** cannot be assumed in RPCs.

#### 2. **Pass-by-Value Requirement**
Because client and server are in **separate address spaces:**
- All **parameters** and **return values** must be **passed by value**.
- Requires:
	- **Marshaling** before sending
	- **Unmarshaling** after receiving
- All transmitted data must:
	- Be **well-typed**
	- Have a **fully defined structure**

#### 3. **Higher Execution Latency**
RPCs are inherently **slower than local calls** because:
- They relyon **underlying communication channels**
- Every call involves:
	- **Request Transmission**
	- **Response Transmission**
**Non-negligible delays** due to **network latenc** and **processing overhead**.
### Naming
A **naming service** is a **central registry** that helps applications **discover and connect** to remote shared resources without needing to know their exact network location.

#### **Purpose of the Naming Service**
- Acts like a **dynamic telephone directory:**
	- Maps a **human-readable name** tp **technical details** (ip address, port)
- **Procides network transparency**
	- Applications just need the **resouce name** and **naming service address** - not the actual ip/port
- **Support dynamic environments**
	- Resources can more or change address

#### **Remote Reference Contents**
When an application asks the naming service for a resource, it gets back a **remote reference**, which includes:
- **IP address** of the host machine
- **Port number** where the server listens
- **Method signatures**
- **Type metadata** for parameters and return values

#### Benefits for Us
- Can **treat remote resources as local**. 
- **Resource discovery and binding** is delegated to the **naming service**
- Easier **scalability and modularity**

## Architecture of RPC

In a RPC system, the work is divided between what we (programmer), must implement and what is automatically generated by the middleware.

![[Addressing_Space_A_SD.png]]
### What the Programmer writes:
- **Main Threads**
- **Shared resources**
- **Service proxy agents**
- **Communication channel wrappers**
We write all the parts that define what the application does and how it interacts.

### What the middleware/runtime generates:

The system takes our interface and types, from that it generates support code that allows remote calls to behave like local ones:
- **Stubs** on the client side 
- **Skeletons or dispatchers** (server-side handlers)
- **Marshaling and unmarshaling code** 
![[Addressing_Space_B_Sd.png]]

### **The Skeleton**

**Skeleton** refers to the automatically generated code running on the server side. It listens for incoming requests and processes them **without interfering** with the main application logic. Let's the server handle RPCs in parallel or asynchronous.

## Code Migration

**Code migration** allows parts of a running application to be **moved between processing nodes** at runtime. Introduces **flexibility, performance optimization, and resilience**.

### **Load Balancing and Performance Optimization**
- Some **nodes have more computation power**
- Migration enables **dynamic relocation** of code to those nodes
- Specialy Beneficial in **heterogeneous systems** (systems that use CPU, GPU, FPGa. many stuff)

### **Fault Tolerance and System Resilience**
- If a node fails or is about to fail, the system can:
	- **Detect the malfunction**
	- **Reassign the affected software components** to healthy nodes
This **prevents application crashes**.

### **Mechanisms Involved in Code Migration**
- **Code state serialization and transfer**
- **Rebinding of dependencies**
- **Resumption of execution**

## Form of Code

When implementing **code migration** in a DS, **what form the migrating code should take** is important. It impacts **portability, compatibility, and execution strategy**.

### Executable Code
Compiled machine code ready for execution
- **Pros:**
	- **Fast Execution**
	- Minimal Runtime
- **Cons:**
	- Needs **similar hardware and OS** on the source and destination nodes
	- **Limited portability**

### Source Code
Human-readable code is transfered and compiled on the dest node
- **Pros:**
	- **Highly portable**
	- Can adapt to **heterogeneous systems**
- **Cons:**
	- Requires a **compiler or interpreter** on dest node
	- **Compilation time**
	- **Security risks**

### Intermediate Code
Special kind of code that works on any computer and gets translated into real machine instructions while the program is running.
- **Pros:**
	- **Good Balance**
	- Execution environments handle most compatibilty concerns
- **Cons:**
	- Requires an **interpreter or virtual machine** on dest
	- **Performance overhead** may be added

## Security Concerns

**Code migration** introduces **security risks**, specially when **code is received from unstrusted or external sources**.

It may attempt to **access local resources**, or contains **malicious logic**. Typical solution is a **Security Manager**

# Synchronization

## Time Concepts

**Global Time** - Time perceived by a universal observer
**Local Time** - Time perceived by local entity
**Logical Time** - Based on information flow, orders events by causality rather than absolute timestamps

### **Second definition**
The **Second** is defined as the time it takes for 9,192,631,770 periods of radiation corresponding to the transition between two hyperfine levels of the ground state of the cesium-133 atom.

## Cristian's Method

Is an **external synchronization technique** used in distributed systems. it assumes the availability of a **UTC time Server**.
![[Cristian's Method.png]]

In this method, a client sends a request to a UTC time server to obtain the current time. The server replies with the time in a standard format. 
Upon receiving the response, the client estimates the round-trip time (RTT) and adjusts its local clock by assuming that the delay was **symmetric —** meaning the time it took for the message to reach the server is approximately the same as the time it took for the response to return. This approach assumes low and relatively symmetric network latency, and that the client’s clock can only move forward or stay the same (monotonic adjustment).

![[Cristian’s Method_decomposition.png]]
1. At **local time** t1 (the client) **sends request** to server.
	- **Message transmission time** is tM1
	- Request is received by server at tUTC1
2. **Server replies** with an estimated **UTC time** tUTC, adjusted to the middle of the **server's processing interval**
3. **Reply is sent** at **UTC time** tUTC 2
	- Reaches client at **local time** t2 after message delay tM2.

Assuming one message experienced **minimum delay (t_{MIN})**  the **worst-case uncertainty** is:
$$ \triangle est = \frac{t_{2} - t_{1}}{2} - t_{MIN} $$

### Berkeley algorithm

Is an **internal clock synchronization** method used when **no UTC time source is available**. Ensures that all **processing nodes** maintain **synchronized local clocks**.

Periodically, one node is elected or designated as the **master node**.
The master **proactively polls** all nodes in the system asking for current time.
After the master:
1. **Calculates the average time offset** between itself and other nodes
2. **Computes the correction** needed for each node to align with the average
3. **Sends back the offset corrections**
Each node then adjusts its clock based on the received correction.

![[Berkeley_algorithm_sd.png]]
### Network Time Protocol

**NTP** is designed specifically for **global synchronization over the Internet**.
#### **Goals of NTP:**
- **Accurate synchronization:** Any computer system connected to the internet is able to adjust its local clock
- **Timely adjustments:** clock corrections at a **frequent rate** prevents drift
- **Resilience to disconnection:** 
- **Security and robustness:**

NTP relies on **hierarchical structure** of time servers, organized into multiple **levels called strata**.

![[NTP_strata.png]]
- **Stratum 1:** These are **primary server** that are directly connected to a UTC source. Serve as root of the hierarchy
- **Stratum 2 and below:** Secondary servers, synchronizes clocks with one or more servers in **stratum n-1**
- **Lateral coordination:** Servers in same stratum can also synch with each other
As u move down the hierarchy the **uncertainty of time info increases.**

# Logical Clocks

Introduced by Lamport, provide a way to order events in distributed sysmtes without relying on synchronized physical clocks. 
**Logical Clocks** capture **causal relationships** and **information flow** between processes.

### What is an Event?
Represents any significant activity within a process. **Message sending** and **message receiving** are important for synch.

## Principles of Event Ordering

Two fundamental principles:
- **Intra-process ordering:** Events occuring in same process, happen in the order perceived by that process 
- **Inter-process message causality:** If a process sends a message to another, the sending event must logically occur before the receiving event.
These principles ensure causality is preserved.

### Classification of Event pairs

They are classified as:
- **Sequential events:** is **possible to determine** that one event happened before the other
- **Concurrent:** No causal or temporal relationship; neither event can be said to have happened before the other

## Lamport’s Happened-Before relation

Leslie Lamport introduced a foundational concpet for event, denoted as:
$$ e ≺ e' $$
This relation defines a **partial order** on events

**Formal Definition**
1. **Intra-process order (local program order):**
	if e and e' occur in the same process pi, and e precedes e', then e ≺ e'
2. **Message causality:**
	if e = send(m) and e' = receive(m), then e ≺ e'
3. **Transitivity:**
	if e≺e' and e'≺e'', then e≺e''

![[ex1_lamport-relation.png]]
**Sequencial Events:**
$$ d \prec j \wedge j \prec k \wedge k \prec c, \Rightarrow d \prec c $$
**Concurrent Events:**

$$ \neg (f \prec c) \wedge \neg(c \prec f), \Rightarrow f \parallel c   $$
$$ \neg (a \prec l) \wedge \neg(l \prec a), \Rightarrow a \parallel l   $$

## Scalar Logical Clock

Each process $p_{i}$ maintains a logical Clock $C_{k}$  that tracks event ordering without relying on physical time. 
1. **Initialization:** 
	The logical clock starts at zero $$C_{k_{i}} = 0$$
2. **Local Event Occurrence:** 
	When a process performs a local event, increments its clock by a constant $$ C_{k_{i}} \leftarrow C_{k_{i}} + a_{i} $$
3. **Message Sending:**
	Before sending a message, the process increments its clock $$ C_{k_{i}} \leftarrow C_{k_{i}} + a_{i} $$
	It then attaches the timestamp $T_{S} = C_{k_{i}}$ 
4. **Message Reception:**
	Upon receiving a message with timestamp $t_{s}$ , the process updates its clock as $$ C_{k_{i}} \leftarrow max(C_{k}, t_{s}) $$
	then it increments the clock for the receive event $$ C_{k_{i}} \leftarrow C_{k_{i}} + \alpha_{i} $$
	This mechanism guarantees that the logical clock values respect Lamport's relation

### Synchronized Replicas

In distributed systems with replicated data, ensuring **permanent synchronization** across all replicas is critical. Without coordination, concurrent write operations can lead to inconsistent states.
#### The Challenge
Write operations must:
- Be applied in the same order on all replicas.
- Be reliably delivered.
- Handle concurrency to avoid divergence.

To achieve this, two main **synchronization requirements** must be met:
1. **Propagation Before Execution**: A process must inform all replicas before applying a change.
2. **Uniform Execution Order**: All replicas must execute operations in the same global order, regardless of arrival time.

These guarantees depend on the system assuming:
- No process failures.
- No message loss.

### Lamport's Algorithm for Synchronization

Lamport proposed a method using **logical clocks** and **priority queues** to ensure consistent update ordering across replicas.
#### Algorithm Overview
1. **Operation Intent**:  
    When a process wants to modify a replica, it timestamps the operation using its logical clock.
2. **Broadcast**:  
    The timestamped message is sent to all other processes, including itself.
3. **Message Handling**:  
    Upon receipt, each process:
    - Updates its logical clock per Lamport’s rules.
    - Inserts the operation into a local priority queue, sorted by timestamp and process ID (to break ties).
4. **Acknowledgment**:  
    Every process acknowledges each received message, even its own.
5. **Execution**:  
    A process executes the operation **only when**:
    - It’s at the head of its queue.
    - It has received acknowledgments from all group members.
This ensures that all processes apply all updates in the **same order**, achieving **strong consistency** across replicas.
## Vector Logic Clock

Lamport’s scalar clocks capture causality only in one direction:  
If `e → e'` then `C(e) < C(e')`,  
but the reverse is not always true. This limitation means scalar clocks can't definitively detect concurrency

### Why Vector Clocks?

To solve this, **Mattern (1989)** and **Fidge (1991)** introduced **vector clocks**. Unlike scalar clocks, vector clocks track both:
- A process’s local event history
- Its partial knowledge of other processes’ histories
This enables accurate detection of:
- **Causality**: `VC(e) < VC(e') ⇔ e → e'`
- **Concurrency**: `VC(e) ∥ VC(e')` (neither precedes the other)

### How Vector Clocks Work

Each process `pᵢ` maintains a vector `Vᵢ` of size `N` (number of processes):
- `Vᵢ[i]`: count of events at `pᵢ`
- `Vᵢ[j]`: latest known count of events at `pⱼ`, based on received messages
#### Vector Clock Update Rules:
1. **Initialization**:  
    `Vᵢ[j] = 0` for all `j`
2. **Local Event at pᵢ**:  
    `Vᵢ[i] += 1`
3. **Sending a Message**:
    - First: `Vᵢ[i] += 1`
    - Then: attach a **copy** of `Vᵢ` to the message
4. **Receiving a Message with Timestamp `ts`**:  
    For all `j`: `Vᵢ[j] = max(Vᵢ[j], ts[j])`  
    Then update local clock: `Vᵢ[i] += 1`

### Comparing Vector Timestamps

Given two vector timestamps `V` and `V′`:
- `V = V′` ⇔ all components equal
- `V < V′` ⇔ all components of `V` ≤ `V′`, and at least one is strictly less
- If neither `V < V′` nor `V′ < V`, then `V ∥ V′` (events are **concurrent**)

![[Vector_logic_clock.png]]

Let `e` and `e′` be events in processes `pᵢ` and `pⱼ`, with associated vector timestamps `Vᵢ(e)` and `Vⱼ(e′)`.

The core principle is $$ e \prec e' \Leftrightarrow V_{i}(e) < V_{j}(e') $$
- $\prec$  denotes the **“happened-before” relation** (as defined by Lamport)
- < denotes the **strict vector clock comparison**.
This equivalence gives vector clocks their power:  
They **fully capture causality** — if and only if `Vᵢ(e) < Vⱼ(e′)`, then `e` causally precedes `e′`.
If neither `Vᵢ(e) < Vⱼ(e′)` nor `Vⱼ(e′) < Vᵢ(e)`, then the events are **concurrent**, with no causal relationship.

# Group Communication

In group communication systems, all processes are treated as equals — there are no privileged roles, and coordination must be achieved through **message passing**, since there is **no shared memory space**. Synchronizing access to shared resources is crucial to avoid race conditions and ensure consistent system behavior.

To manage shared object access, a **guardian process** (or coordinator) is introduced. This design is a natural extension of the client-server model, where the guardian serializes access by queuing requests and granting them one at a time.
#### How the Access Protocol Works:
- When a process `pᵢ` wants to access a shared object, it sends a _request_ to the guardian `pₙ`.
- If the object is free, the guardian replies with _grant access_. Otherwise, it queues the request.
- Once granted, `pᵢ` can proceed. After finishing, it sends a _release_ message.
- The guardian then grants access to the next process in the queue, if any.
#### Key Considerations:
- **Message Overhead**: Each access involves three messages — request, grant, release.
- **Not Fully Decentralized**: The guardian creates a centralized control point.
- **Single Point of Failure**: If the guardian crashes, no process can proceed, halting coordination.
This approach ensures **mutual exclusion** but at the cost of added overhead and a critical dependency on a single coordinator.

#### Logic Ring
Processes are organized in a circular structure with strict communication rules and controlled access to shared resources.
#### **Structure & Communication Rules**
- **Ring Configuration**: Processes are logically connected in a circle (closed comm loop).
- Each process $p_{i}$​ can:
    - **Receive messages** from $p_{(i-1)}|N|$ (its predecessor),
    - **Send messages** to $p_{(i+1)}|N|$ (its successor),  
        where N is the total number of processes.
#### **Token Passing Mechanism**
- A special message called a **token** circulates around the ring.
- Only the **process holding the token** is allowed to access the **shared resource** (e.g., a critical section, file, or device).
- Once done, the process passes the token to its successor.

![[Logic_ring_SD.png]]
#### **Token-Based Access Protocol (Ring Topology)**
**Mechanism**:
- A **single token** circulates among all processes.
- Only the process holding the token can access the shared resource.
**Requesting Access**:
- If process $p_{i}$ **needs access**:
    - It **waits for the token**.
    - Upon receiving the token, it accesses the object.
    - After finishing, it sends the token to $p_{(i+1)}|N|$​.
- If $p_{i}$​ **doesn't need access**:
    - It **immediately forwards the token** to the next process.

- **Message Overhead**  
    A message is exchanged in every cycle, regardless of whether any process actually needs to access the shared object. This means there's a constant communication cost, even when the system is idle.
- **Efficiency in Small Groups**  
    The protocol performs very well when the number of participating processes is small. The token circulates quickly, leading to minimal waiting time for access.
- **Scalability Limitation**  
    As the number of processes increases, the system becomes less responsive. A process may wait a long time to receive the token, even if no other process is actively using the shared resource. This delay is due to the strict circulation order of the token, which cannot be bypassed.

### Mutual Exclusion with Logical Clocks
**Ricart and Agrawala (1981)** proposed a distributed algorithm for achieving **mutual exclusion** among _N_ processes. The key idea is to 
**order access requests** using **Lamport logical clocks** to avoid conflicts and ensure fairness.
#### Consensus on Access Order
- Every process in the system maintains agreement on the **total order** of access requests.
- This ordering ensures a **global consensus** when deciding **who gets access** to a shared resource.
- Requests are only granted when:
    - The request is the **earliest** in the logical clock order.
    - All other processes have either:
        - Not made a request, or
        - Sent a reply acknowledging the request.

![[Total Ordering of Events_sd.png]]

Every message includes a **logical timestamp** of the sending event. Upon receiving a message, a process **updates its logical clock** based on **Lamport's clock rules** (i.e., take the max of local and received timestamps + 1).
### Extended Timestamps for Total Order
- To achieve **total ordering**, each access request is tagged with an **extended timestamp**:  
    `(ts(m), id(m))`  
    Where:
    - `ts(m)` = logical timestamp of the message
    - `id(m)` = sender’s process ID
- This pair helps:
    - Break **ties** when timestamps are equal.
    - Establish a **deterministic total order** of events.
#### Message Overhead
- Each critical section access requires `2(N−1)` messages:
    - `(N−1)` **request** messages
    - `(N−1)` **grant** (reply) messages
#### Efficiency vs Scalability
- **Efficient for small groups**: fewer messages and fast coordination.
- **Scalability limitation**:
    - Message count increases quickly with **larger process groups**.
    - Can become **communication-intensive**.
#### Group-Based Permission Model (Maekawa, 1985)
- Mutual exclusion without contacting **all** processes.
- Each process belongs to a **subset (group)** of processes.
- To access a shared object, a process needs **permission from all group members**.
#### Ensuring Mutual Exclusion
- Groups are **not disjoint**—they must **intersect**.
- This guarantees:
    - No two processes can **simultaneously** enter the critical section.
    - There's always at least one process common to conflicting requests.
#### Permission by Voting
- Access is granted using a **voting mechanism**:
    - A process can enter the critical section **only after receiving all votes** from its group.
    - Each process can vote for **only one** requester at a time.

### Minimizing the Number of Messages

To reduce communication overhead in distributed mutual exclusion, an approximate voting group structure can be used.
- Processes are organized in a √N × √N matrix.
- Each process belongs to exactly **two voting groups**:
    - One corresponding to its **row**
    - One corresponding to its **column**
This design ensures that:
- Every pair of processes shares at least one group (intersection property)
- Total message complexity per access becomes **O(√N)** instead of O(N)
Each access involves approximately **3 × O(√N)** messages **(requests, grants, and releases within the groups)**, making the protocol much more **scalable and efficient** for large systems.

#### Correctness Issue: Potential Deadlock
While Maekawa’s method is efficient, it **can lead to deadlock** due to circular wait conditions.
##### Solution: Enforcing Order
To prevent deadlock:
- Assign a **total order** to all requests using Lamport timestamps or logical clocks.
- Each voter grants permission only to the **earliest request**.
- Processes maintain a **priority queue** of pending requests ordered by timestamp.
When a process finishes using the shared resource:
- It releases its vote to the next process in its queue.
An alternative safeguard involves a **fallback mechanism**:
- Use a **centralized coordinator** or a **token-based system** during high contention to guarantee forward progress.

### Election Procedure in Symmetric Process Groups
A **leader** may need to be elected to perform a task (e.g., coordination, resource management). Even though all processes are peers, the system must satisfy key properties for correctness:
- **Termination**: The election finishes in a finite number of steps.
- **Unambiguity**: Only one process is elected.
- **Consensus**: All processes agree on the elected leader.
These conditions ensure that the election process is both reliable and deterministic, even in a fully distributed setting.

##### System Assumptions for Election Protocols
Before running any election algorithm, certain conditions about the system are assumed:
- **Fixed Process Group**:  
    The number of processes is known and does not change.
- **Process States**:  
    A process is either:
    - _Alive and executing_, or
    - In _catastrophic failure_ (i.e., fully unresponsive or crashed).
- **Message Transmission Timing**:  
    Message delivery time is bounded, allowing the use of **timeouts** to detect failures.
- **Communication Reliability**:  
    Messages can be **lost**, so protocols must include:
    - Retransmissions
    - Acknowledgments

##### Ring-Based Election: Initialization
- Initially, **no election is in progress** and all processes are in the **non-participant** state.
- **Any process** can start an election:
    - Sets its state to _participant_
    - Sends a `start election` message to the **next process in the ring**
    - Message includes the sender's **process ID**

##### Handling start election Messages
When a process receives a `start election` message, it compares the message's ID to its own:
- **Case 1: message ID < own ID**
    - Forwards the message unchanged to the next process
    - Becomes a _participant_ (if not already)
- **Case 2: message ID > own ID**
    - If not a participant:
        - Replaces message ID with its own ID
        - Forwards it to the next process
        - Sets state to _participant_
    - If already a participant:
        - Discards the message to reduce traffic
- **Case 3: message ID = own ID**
    - The process has received its own ID back — it is now the **elected leader**
##### Leader Announcement
Once a process has been elected:
- It sends an `elected` message containing its own ID to the next process.
### Handling `elected` Messages
- Every process receiving the `elected` message:
    - **Resets participation state** to _non-participant_
- **If leader ID ≠ own ID**:
    - Stores the new leader’s ID
    - Forwards the message
- **If leader ID = own ID**:
    - Discards the message — the election process has completed
##### Failure and Recovery
###### Failure Detection
- Use **timeouts** to detect failures:
    - If no message is received from the next process within a given time, assume it has failed.
###### Bypassing Failed Processes
- On detecting failure:
    - **Skip the failed process** and forward the message to the next alive process
    - This requires:
        - Maintaining a **local view** of the ring
        - Or relying on an **external failure detector**

### Failure and Recovery in Election Protocols
Handling dynamic failures and recoveries requires extra mechanisms to ensure correctness and consistency across the system.
#### Rejoining After Recovery
When a previously failed process comes back online:
- It must **announce its return** to the group.
- The **ring structure must be updated** to include the recovered process.
- In some cases, this may **suspend the current election** and trigger a restart with the updated topology.
#### Reliable Communication Layer
To cope with unreliable channels:
- Every election-related message should be **acknowledged (ACKed)**.
- If no ACK is received within a timeout:
    - The message is **resent**, with a retry limit to avoid infinite retries.
#### Timeout and Restart Mechanisms
- If a process suspects message loss or election stalling:
    - It can **restart the election**, potentially using a **higher process ID** to resolve conflicts.
- Care must be taken to avoid **concurrent elections**, which could lead to inconsistencies.
#### General Election Initialization (Alternate Form)
A different version of the election algorithm (e.g., Bully-style) works as follows:
- Initially, all processes are in the **no participant** state.
- **Any process** can trigger an election:
    - Marks itself as _participant_
    - Sends a `start election` message (with its own ID) to **all processes with lower IDs**
##### Handling Messages
- **On receiving a `start election` message**:
    - Reply with an **acknowledge message**
    - If in `no participant` state:
        - Become a participant
        - Forward a new `start election` message to lower-ID processes
- **On receiving an `acknowledge` message**:
    - The sender waits for an `elected` message to learn the leader's identity
- **If no `acknowledge` is received within timeout**:
    - The process assumes it's the **lowest-ID live process**
    - Declares itself **leader**
    - Sends an `elected` message to all processes

### Garcia-Molina’s Election Algorithm (1982)
This algorithm was originally designed for **static, failure-free environments**, assuming:
- A **known set of processes**
- **Reliable communication**
- No failure **during** election
#### Extensions for Fault Tolerance
##### Process Failure
- Use **timeouts and heartbeats** to detect failures.
- If the **coordinator fails**:
    - The **highest-ID process** detecting the failure initiates a new election.
    - It queries higher-ID processes; if none respond, it **elects itself**.
##### Failure Propagation
- Once a failure is detected:
    - Processes **broadcast the updated membership** list to maintain consistency.
##### Process Recovery
When a failed process returns:
- It **announces itself** to the group.
- Sends a **“Who is leader?”** query to learn the current leader.
- If its ID is **higher than the current leader**, it may initiate an election (depending on policy).

### Robust Communication Handling
#### Message Reliability
- Use **ACKs and retransmissions** for critical messages:
    - `start election`
    - `acknowledge`
    - `elected`
#### Election Recovery
- If a process **waits too long** after starting an election without success:
    - It **retries** the election
    - Use **election IDs or round numbers** to avoid duplicate or conflicting elections
### Enhancements for Robustness
- **Membership Service**:
    - Use a distributed or centralized service to track **active processes**
    - Ensures all nodes are informed of the current system view
- **Persistent State**:
    - Election-related state (e.g., current leader, participation flag) can be stored persistently
    - Allows recovery processes to resume cleanly after a crash