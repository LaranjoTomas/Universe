# Vehicular Network

## Cooperative Awareness Messages (CAM)

They are **Periodic** (1Hz -10Hz) and **contain information about the station, such as the position and speed**. These messages create and maintain awareness of vehicles using the road network or RSUs. These messages are used **to support basic safety applications**, such as **collision avoidance**, **lane merge coordination**, and **intersection management**, by enabling vehicles to track each other's presence and motion.

## Decentralized Environmental Notification Messages (DENM)

They are asynchronous messages that **create and maintain awareness about a road event** such as its type, position and the history of the event. Unlike CAMs, **DENMs** are **generated as events occur**. These messages are used **to warn nearby vehicles about hazards**, such as **accidents**, **obstacles**, **sudden braking**, or **dangerous weather conditions**, enabling timely and proactive reactions.

## Vulnerable Road User Awareness Messages (VRU & VAM)

They are periodic messages like CAMs to create and maintain awareness on VRUs (Vulnerable Road Users), and support the risk assessment. **VAM** has advantages over CAM messages because of its **flexibility in terms of specifying the VRU type and situation** (not possible with CAMs), they may be pedestrians, cyclist, motorcyclists or animals. These messages increase the **digital visibility** of smaller, more vulnerable road users to nearby vehicles, helping to reduce accidents due to blind spots.

## Cooperative Perception Message (CPM)

They are **periodic** messages **between stations** to **broadcast information** about the current environment perceived by 1 or more sensors. CPMs messages from sensors from a vehicle can be used by VRUs and infrastructure to exchange information obtained from their surrounding, improving awareness. They are just for perception, for example, extending a vehicle perception range beyond its line-of-sight (around corners).

## Signal Phase and Timing (SPAT)

Creates open interface for two-way communication between traffic signal controller and mobile devices. They have the state of the traffic signals in a intersection/etc. Used in cooperation with a **map**.

## MAP (MAP)

Geometric layout of intersection/etc. 
## Manoeuvre Coordination Message (MCM)

These messages are used for **planned maneuvers** of one or more vehicles in the network. They **include the intended maneuvers and one or more desired trajectories**. Generated continuously between (1Hz-10Hz) depending on the context. 
**Examples:**
- Goto Maneuver
- Idle Maneuver
- Follow Path Maneuver
- Follow Trajectory
- Scheduled Goto
- Stop Maneuver
- Maneuver Done
- Teleoperation Maneuver
- Teleoperation Done

Can be transmitted by the vehicles and/or infrastructure nodes to coordinate a maneuver.
### Maneuver Cooperation Service

This service serves as the orchestrator and facilitator, responsible for producing and managing the distribution of MCMs. Supports the driving automation functions of connected cooperative automated vehicles.
![[image 8.png]]


## ITS-G5

**Developed for vehicle-to-vehicle communication**, **adapted for latency-critical** V2X communications.
Faces many challenges, the safety of vehicle communication rely heavily on periodic broadcast of basic safety messages which contain positions, velocities and other info. These messages with the PHY layer overheads typically around 300 bytes with full security, which can lead to channel congestion in dense vehicular environments. **Lackhandshakeke/ACK in delivering broadcast frames,*doesn'tesnt support QoS** 


## Cellular-V2X (C-V2X)

**C-V2X** defines new interfaces called **PC5** for **V2V, V2I** communication. **V2N** is still over the legacy **LTE Uu** interface and provides over the top cloud services.

It defines two **Complementary Transmission Models**:
- **Direct safety communication independent of cellular networks**
- **Network communications for complementary services**

**Direct communications** via **PC5 interface**. Builds upon LTE Direct device2device design with enhancements.
**Network communications** via **Uu interface**. Uses LTE to broadcast messages from a V2X server to vehicles and beyond. Vehicles can send messages to the server via unicast.

![[image 7.png]]

### 1. C-V2X with 4G (LTE-V2X)
#### Advantages
- Commercially available today (existing LTE infrastructure).
- Supports **both direct communication (PC5) and network-based (Uu)**.
- Suitable for **basic safety applications** (e.g., emergency braking alerts).
- Compatible with both Mode 3 (network-assisted) and Mode 4 (autonomous, out-of-coverage).
- **Lower deployment cost compared to 5G.**
#### Disadvantages
- Relatively **high latency** (~30–50 ms), unsuitable for real-time critical maneuvers.
- **Lower bandwidth** and connection density (issues in crowded scenarios).
- **Limited reliability** for ultra-critical applications (e.g., coordinated braking).
- **Insufficient for advanced services like cooperative driving or teleoperation**.
- **Does not support network slicing or URLLC** (Ultra Reliable Low Latency Communication).
### 2. C-V2X with 5G (NR-V2X)
#### Advantages
- **Ultra-low latency** (<5 ms) — ideal for real-time critical communications (e.g., advanced platooning).
- **High reliability** (>99.999%) for safety and automation.
- **High communication capacity** — suitable for dense urban environments with many connected vehicles.
- Supports advanced functionalities:
  - **Automated cooperative platooning**
  - **Teleoperation**
  - **Edge computing**
  - **Network slicing**
- Greater **scalability** and **support for high-speed vehicles** (>500 km/h).
#### Disadvantages
- **Requires 5G infrastructure**, which is still being deployed (not yet widely available).
- **Higher deployment cost.**
- I**ncreased complexity** in network resource management and system configuration.
- 5G-compatible equipment (modems/OBUs) may still be more expensive.
## Final Note

While **4G LTE-V2X** is sufficient for **basic and safety-related applications**, only **5G NR-V2X** enables the full potential of C-V2X, especially in **critical use cases** like **automated platooning**, **mass coordination**, and **cooperative autonomous driving**.

# QoS and security

## Evaluate TCP

### TCP in Wireless and Ad-hoc Networks

TCP was originally designed for **reliable, wired networks**, where:
- **Packet loss = congestion**
- **Fixed routes** and **low error rates** are expected

This makes **TCP unsuitable** for:
- **Ad-hoc networks** (e.g., MANETs, VANETs)
- **Wireless environments** with high variability
### TCP Concepts 
**Conventional TCP:** Tahoe, Reno, NewReno  
**Control parameters:**
- **`cwnd` (Congestion Window):** Limits # of packets in-flight
- **`ssthresh` (Slow Start Threshold):** Marks the transition to congestion avoidance
**Loss Detection:**
- **3 duplicate ACKs** → triggers fast retransmit
- **Timeout expiration** → triggers slow retransmit (inefficient)
**Congestion Control Phases:**
1. **Slow Start:**
    - `cwnd` starts at 1
    - Increases **exponentially**
2. **Congestion Avoidance:**
    - `cwnd` increases **linearly**
3. **Fast Retransmit & Recovery:**
    - Triggered by **3 duplicate ACKs**

### Differences in Wireless & Ad-Hoc Networks
1. **Mobility**
    - Routes may change or disappear → **unstable links**
2. **High Bit Error Rate**
    - Losses often due to **noise/fading**, not congestion
3. **Unpredictability**
    - **RTT**, bandwidth, and timeouts are harder to estimate
4. **Contention for Airtime**
    - Both **intra-flow** and **inter-flow** contention occurs
5. **Poor Long-Path Performance**
    - TCP throughput **drops sharply** beyond 4 hops

### Why TCP Fails
#### Misinterpretation of Events:
- **Route failures as Congestion**
    - Causes unnecessary rate reduction
- **Wireless errors as Congestion**
    - Triggers congestion control unnecessarily
#### Delay Spikes:
- Lead to **false retransmissions** due to timeout miscalculation
#### Retransmission Loss:
- If a retransmitted packet is lost, performance degrades badly
####  Overall Effects:
- Over-conservative behavior
- High overhead from duplicate retransmits
- Long recovery time
- Inaccurate RTT and congestion estimates

## TCP Performance in Ad-Hoc and Wireless Networks
### Why TCP Performs Poorly
TCP was originally designed for wired networks and assumes that all packet losses are caused by **congestion**. In wireless and ad-hoc environments, this assumption is invalid due to:
- **Mobility**: Dynamic topology changes break routes.
- **High Bit Error Rates**: Packets are lost due to noise, not congestion.
- **Unpredictability**: RTT and bandwidth are highly variable.
- **Contention**: Intra/inter-flow packet competition reduces throughput.
- **Multihop Limitations**: Throughput drops significantly after 4 hops.

TCP misinterprets losses due to route failures or wireless errors as congestion, leading to:
- Reduced sending rates.
- Unnecessary timeouts and retransmissions.
- Poor performance, especially when retransmitted packets are lost.
## TCP CUBIC
### Overview
TCP CUBIC is a modern congestion control algorithm optimized for high-speed networks and designed to be **RTT-independent**. It uses a **cubic function of time** since the last congestion event to determine the congestion window size. 
### Key Properties
- **Window Growth**:
    - `W(t) = C(t - K)^3 + W_max`
    - Where:
        - `W(t)`: cwnd size at time `t`
        - `W_max`: cwnd before the last congestion event
        - `C`: a scaling constant
        - `K = \sqrt[3]{W_max \cdot \beta / C}`
- **Window Reduction**:
    - On packet loss: `W(t*) = W_max`, then `W(t) = W_max * (1 - \beta)`
    - For TCP CUBIC: `\beta = 0.3`
    - For QUIC using CUBIC: `\beta = 0.15`
### Advantages
- Does not rely on RTT for cwnd growth.
- Better suited for lossy, high-delay environments (P2P).
- Gradual growth and aggressive probing improve performance over large paths.


## TCP VEGAS
### Overview
TCP Vegas is a **delay-based** congestion control algorithm. Unlike loss-based variants, it tries to detect congestion **before** packet loss occurs, and instantly it decreases the window size. So, **TCP Vegas** **handles the congestion without any packet loss** occuring.
### Congestion Detection
- Uses the difference: `diff = Expected Rate - Actual Rate`
- Expected Rate = `Window Size / BaseRTT`
- Actual Rate = `Window Size / CurrentRTT`
### Congestion Control Logic
- If `diff < α`: increase `cwnd` linearly.
- If `diff > β`: decrease `cwnd` linearly.
- If `α < diff < β`: maintain `cwnd`.
### Additional Features
- **Modified Slow Start**:
    - Exponential growth every other RTT only.
    - If `diff > c`, Vegas exits slow start and changes to congestion avoidance.
- **New Retransmission**:
    - Tracks transmission time.
    - When DUPACK arrives, checks if it's expired
    - Uses timestamp instead of waiting for 3 DUPACKs to trigger retransmission.

### Benefits
- Handles congestion **without packet loss**.
- Smoother network behavior.
- Better suited for stable but variable-delay environments.

TCP Vegas is designed to **sense congestion** in the network **before any packet loss occurs**. This proactive approach aims to **handle congestion without actual packet loss** by instantly decreasing the window size

**TCP Vegas would perform well in stable wire-line networks where the RTT is consistent and predictable.**
## QUIC

QUIC is a modern transport protocol built over **UDP** with integrated **TLS** and **multiplexing** features. 
### Features
- Combines transport and cryptographic handshake.
- Uses **bidirectional** (endpoints can send data) and **unidirectional** (single endpoint sends data) streams (these are ordered sequences of bytes).
- Eliminates head-of-line blocking across multiple streams.
- Handles loss recovery per stream.
### Flow Control
- **Stream-level**: Prevents any one stream from consuming a full buffer, limits amount of data that can be sent on each stream.
- **Connection-level**: Limits total data across all streams, limits total bytes of stream data sent on all streams.
### RTT Estimation
- Uses **unique packet numbers** (monotonically increasing).
- **Measures RTT** via timestamps on sent/ACKed packets.
- **Packet Loss** detected through lack of ACK packets.
- Congestion control is decoupled from RTT, uses **CUBIC**.
## TCP-BuS: Buffering and Sequence Handling

- Uses **route failure notifications** to detect disruptions.
- Intermediate nodes **buffer** pending packets.
- Sender **doubles RTO** on route failure.
- Special routing messages handle rerouting.
### Pros
- Reduces timeouts and retransmissions.
### Cons
- Requires to be modified routing protocol.
- Needs cooperation from intermediate nodes.
### Use Case:
Can be used to improve TCP performance in wireless and ad-hoc networks, specifically in scenarios involving **route failures**. 
- **Detection of Route Failures:** It uses explicit route failure notifications (errors) to identify when a path has broken.
- **Buffering at Intermediate Nodes:** When a route failure is detected, intermediate nodes buffer the pending packets that were in transit. This prevents immediate packet loss that would otherwise occur if packets were simply dropped.
- **Adjusting Retransmission Timeout (RTO):** Simultaneously, the TCP sender doubles its retransmission timeout (RTO) value. This allows more time for a new path to be found before unnecessary retransmissions are triggered.
- **Path Re-discovery:** The system makes use of special messages, such as localized query and reply, which are modified to carry TCP connection and segment information, to find a new path.

TCP-BuS is explicitly designed to improve TCP performance in **wireless and ad-hoc networks where route failures are common**. The sources note that 6G will feature p2p relationships, which often imply ad-hoc network characteristics where route failures can occur
## QoS in UDP
### **IntServ-Like** Mechanisms
- Resource reservation protocol estimates and reserves resources at each node on the path.
- Issues:
    - Estimating resources is hard.
    - Bandwidth fluctuates with mobility.
    - Re-reservation is needed on route change.
### **DiffServ-Like** Mechanisms
- Class-based service. Instead, of making explicit resource reservations for each flow, it simplifies it by categorizing traffic based on its needs.
- Issues:
    - Hard to enforce admission control.
    - Class overload due to lack of centralized ingress nodes (point where traffic flows enter a particular network segment or domain).
    - Hard to maintain assurances
### **QoS Routing**
- Routing metrics include QoS requirements.
- Can inform a **source node** of the **bandwidth and QoS availability** of a destination node and of the path to the destination node
- Challenges:
    - Overhead
    - Route maintenance
    - Mobility responsiveness
    - Guaranteeing reserved resources
Great for ad-hoc networks that need to minimize packet loss in stable connections.
### **QoS for AODV** 
- RREQ and RREP are extended with QoS info.
- Nodes only forward RREQs if they can satisfy QoS requirements.
- Routing tables are extended to include:
    - Maximum delay
    - Minimum bandwidth
    - Sources requesting delay/bandwidth
#### Loosing QoS
If a node detects that the QoS cannot be maintained anymore, it originates an ICMP QoS_LOST message to all depending nodes

### **QoS-OLSR**
- MultiPoint Relays (MPRs) are selected based on QoS:
    - **AB**: Available Bandwidth
    - **PoNOS**: Neighbor diversity
    - **LW**: Lane weight (traffic alignment)
- Prioritizes stable, high-quality paths.

# Security
## Symmetric vs Asymmetric Cipher
### Symmetric Cipher
- Fast and secure for privacy/integrity.
- Larger key length = more security
- Requires shared key à priori → poor scalability.
### Asymmetric Cipher (Public-Key Encryption)
- **No prior shared key needed.**
- Scalable but computationally expensive.
- Computationally intensive, may require certificate authority and the private key have to be confidential
Vital for some systems duo to:
- **Identity authentication**
- **Secure Channel Establishment**
- Non-repudiation
- Public key distribution

## Partial Key systems

Partial Key Systems are cryptographic schemes where **each participant holds only part of a cryptographic key**, and the full key can only be reconstructed or used **when multiple parts are combined**.

- The **secret (key)** is divided into multiple **partial keys** (or "shares").
- A certain number (threshold) of partial keys must be combined to **reconstruct** or **use** the full key.
  - Example: In a (3,5) scheme, any **3 out of 5** participants can reconstruct the key.
- If fewer than the threshold number of keys are available, the secret remains **inaccessible**.

###  Advantages
- **Improved security**: No single party holds the entire key.
- **Fault tolerance**: System can function even if some parties are unavailable.
- **Distributed trust**: Reduces risk of insider threats and key compromise.
- **Useful in sensitive environments**: e.g., joint control of nuclear launch codes, corporate governance, blockchain wallets.
### Disadvantages
- **More complex key management**.
- **Increased overhead** in coordination and communication.
- **Performance impact** if used in real-time systems.

#### Num sistema de chaves parciais, identifique 2 problemas de segurança que podem acontecer. Justifique.
**R:** If a certain number of participants demand for the reconstruction of the key between themselves, it would be possible for them to recover the secret in its entirety without collective authorization. Even if an attacker doesn't meet the minimum numbers of partial keys necessary, it can gradually obtain multiple shared keys to reveal the secret over a long time.


## Diffie-Hellman Key Exchange
- Establishes a shared session key over public channels.
- Based on agreed `p` and `g`, users compute and exchange values to derive the same session key.
![[image 9.png]]

## RSA (Rivest-Shamir-Adleman) Public-Key Encryption
- Each user generates public/private key pairs.
- Public key used for encryption, private for decryption.
- Secure if large prime numbers are chosen, these numbers are randomly generated and used to create the keys.

## Key Management in Ad-Hoc Networks
- Challenges: No infrastructure to send key information to nodes, mobility-induced unstable network topology, physical vulnerability.
#### Building Trust in Ad-Hoc Networks
In ad-hoc networks, where devices don't know each other and there's no central authority, building trust is crucial for managing keys. 
Trust is built through distributed methods:
- **Reputation-Based Trust:** Nodes observe each other's behavior. Honest actions build good reputations, making those nodes more trusted. However, this can be slow to establish and vulnerable to initial "good behavior" by malicious nodes.
- **Pre-existing Knowledge (Bootstrapping):** A small set of initially trusted nodes can introduce and authenticate others, extending a "web of trust."
- **Trust Paths:** If Node A trusts Node B, and Node B trusts Node C, Node A might indirectly trust Node C. The challenge is verifying the strength of these paths in a dynamic network.
- **Out-of-Band Verification:** Sometimes, trust can be established through physical interaction, like scanning a QR code with a public key in person before digital connection.
##### In SOPKM (Self-organized Public Key Management)

In this model, users emit certificates based on personal acquaintance. One certificate is a connection between nodes and its public key, contains the public key, the identity of the node and the signature of the one who emited it (issuer). This certificates are stored and distributed by the users themselves. 
##### In SSAWN (Self-securing AD-hoc Wireless networks)

This method utilizes a local model of trust, a node is considered trustworthy if a certain number of entities which are trustworthy claim him trusthworthy during a specified time. Once a node is trusted by the network, he is globaly accepted has a trustworthy node. SSAWN utilizes a mechanism of shared secret where the global secret key (SK) which is distributed to all nodes. Whichever "k" nodes that have a part of the secret build a distributed certificate authority. 

### SOPKM (Self-Organized Public Key Management)
- Users issue and store certificates locally.
- Trust graph built by users signing each other’s public keys.
- Uses:
    - Certificate exchange via hash IDs.
    - Certificate update before expiry.
    - Revocation by issuer.
    - Conflict resolution by comparing bindings.
- Asks from neighbors for certificates it doesn't have
**Vertices:** public keys of some nodes
**Edges:** public keys certificates issued by users
#### Certificate Exchange
Each node `u` **multicasts its subgraphs** (`Gᵤ` and `Gᵤ'`) to its **1-hop physical neighbors**.
- Instead of sending full certificates, the message includes only **unique identifiers** (e.g., hash values) of the certificates.
- Upon receiving this message, neighbors **reply with the hash values** of the certificates in their current (updated and non-updated) repositories.
- Node `u` then **cross-checks the received hashes** against its own repository and **requests only the missing certificates** from its neighbors.
#### Certificate Update

Before a certificate expires, its issuer issues an updated version of the same certificate with an extended expiration time. 
**Each node issues certificates updates periodically**.
Needs time synchro of the nodes. **Certificates updates will be exchanged regularly**.

#### Certificate Revocation

Each user can revoke a certificate that it has issued if:
- User-key binding (believes) in that certificate is **no longer valid**
- Own private key is **compromised**
**Certificate revocation:**
- **Explicitly** - issues explicit revocation statement, only needs to send it to **nodes that it regularly updates**.
- **Implicitly** - Expiration time of the certificate

#### Malicious Users
The **certificate exchange mechanism** allows nodes to gather **almost all certificates** from the graph `G`.
- Nodes **cross-check user–key bindings** in the certificates they hold to **detect inconsistencies**:
    - **Same username** bound to **different public keys**
    - **Same public key** bound to **different usernames**
- When conflicts are detected, **additional certificate exchanges** may be required to **resolve inconsistencies**.

## SSAWN (Self-Securing Ad-Hoc Wireless Networks)
**Goals:**
- Achieve high security assurance
- High success ration
- Efficient communication
**Localized trust model:** an entity is trusted if any `k` trusted entities claim so within a certain time period
- `k` entities typically among the **entity’s one-hop neighbors**
- Once a node is trusted by its **local** community, it is **globally** accepted as a trusted node.
- Otherwise, a locally distrusted entity is regarded as untrustworthy in the entire network.
### Shared secrets
It uses RSA asymmetric keys for encryption mechanism. A Global secret key (SK) and the corresponding Public Key (PK), the **SK** is **distributed** among the nodes, any **`k`** **nodes** holding a **partial secret** form a distributed Certificate Authority. 
**SK is used to sign certificates** for all nodes in the network. A certificate signed by SK can be verifies by the well-know public key **PK**. 
Each node has a part of the secret:
- **Unique ID**, from node's address
- Mechanism for local detection of misbehaving nodes
- At least K one-hop neighbouring nodes
- Key pair for each node (public and secret keys)

• **Partial secret key as a function of nodes IDs**.
• **Node wanting to use the distributed CA**.
• A misbehaving or broken node will be unable to renew its certificate.
• A valid certificate represents the trust from a coalition of k nodes

## Reputation Aproaches

Reputation systems are used to **evaluate the reliability of nodes** in a distributed and self-organized manner.
### Goals:
- **Favor routing and communication** through **high-reputation nodes**
- **Minimize interaction** with **misbehaving or unreliable nodes**
- **Protect network traffic** from threats such as selfish or malicious behavior
### Behavior and Reputation Assessment
Nodes evaluate each other based on **packet forwarding behavior**, such as:
- **Correct forwarding**
- **Altering packets**
- **Injecting unauthorized packets**
### Key Points:
- **Observation-based evaluation** of neighboring nodes
- **Reputation info is periodically exchanged** among neighbors
- Nodes need **network interfaces in promiscuous mode** to monitor nearby traffic
- **Level of trust** is assigned to the source of reputation reports
### Classification of Nodes
In a **decentralized, self-organizing system**, nodes are categorized based on behavior:
- **Friendly:** Participate as expected
- **Selfish:** Refuse to forward packets
- **Malicious:** Inject or misroute traffic
### Mechanisms:
- **Each node observes and evaluates** the behavior of its neighbors
- **Reputation is used to select** the most **reliable and secure paths**
- **Cooperation is encouraged** through:
    - **Rewards** for obedient behavior
    - **Sanctions** for intolerable actions
### Reputation
Total Reputation is a combination of:
- **First-hand reputation**: Direct observations from neighbors
- **Second-hand reputation**: Information received from other nodes
#### Behavior Modeling:
- A node’s behavior follows a **distribution**:
    - Based on the **number of observed packets from a node**
    - Evaluates how many packets were **not transmitted or were altered**
    - A node is considered **friendly** if the **probability of well-transmitted packets** exceeds a threshold `x` (Bayesian approach)
#### Collaborative Monitoring:
- Nodes **exchange first-hand reputation data** with neighbors
- A **deviation test** helps detect **false reports**:
- If the probability `x` observed across multiple nodes deviates more than a threshold `d`, the report is considered untrustworthy
### Reputation into the normal operation of the network
Reputation influences several core functions:
- **Path Selection**:
    - Routes are chosen based on the **reputation of candidate nodes**
- **Certification Graph Membership**:
    - Only nodes with **high reputation** are allowed to join
- **Key Management**:
    - Nodes selected to receive **key parts or key pairs** must have **high reputation**
#### Dynamic Reputation:
- A node’s reputation can **change over time**
- Nodes may be **temporarily inactive** or disconnected without necessarily being malicious