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
![[image.png]]


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

### TCP Fundamentals (Conventional)
**Variants:** Tahoe, Reno, NewReno  
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

### Challenges in Wireless & Ad-Hoc Networks
1. **Mobility**
    - Routes may change or disappear → **unstable links**
2. **High Bit Error Rate**
    - Losses often due to **noise/fading**, not congestion
3. **Unpredictability**
    - RTT, bandwidth, and timeouts are harder to estimate
4. **Contention for Airtime**
    - Both **intra-flow** and **inter-flow** contention occurs
5. **Poor Long-Path Performance**
    - TCP throughput **drops sharply** beyond 4 hops

### Why TCP Fails
#### Misinterpretation of Events:
- **Route failure Congestion**
    - Causes unnecessary rate reduction
- **Wireless losses Congestion**
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
- Better suited for lossy, high-delay environments.
- Gradual growth and aggressive probing improve performance over large paths.
## TCP VEGAS
### Overview
TCP Vegas is a **delay-based** congestion control algorithm. Unlike loss-based variants, it tries to detect congestion **before** packet loss occurs.
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
    - If `diff > γ`, Vegas exits slow start early.
- **New Retransmission Strategy**:
    - Tracks transmission time.
    - Uses timestamp instead of waiting for 3 DUPACKs to trigger retransmission.
### Benefits
- Handles congestion **without packet loss**.
- Smoother network behavior.
- Better suited for stable but variable-delay environments.
## QUIC
QUIC is a modern transport protocol built over **UDP** with integrated **TLS** and **multiplexing** features.
### Features
- Combines transport and cryptographic handshake.
- Uses **bidirectional** and **unidirectional** streams.
- Eliminates head-of-line blocking.
- Handles loss recovery per stream.
### Flow Control
- **Stream-level**: Prevents any one stream from consuming a full buffer.
- **Connection-level**: Limits total data across all streams.
### RTT Estimation
- Uses **unique packet numbers** (monotonically increasing).
- Measures RTT via timestamps on sent/ACKed packets.
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
## QoS and UDP
### IntServ-Like Mechanisms
- Reserve resources per flow.
- Issues:
    - Estimating resources is hard.
    - Bandwidth fluctuates with mobility.
    - Re-reservation is needed on route change.
### DiffServ-Like Mechanisms
- Class-based service.
- Issues:
    - Hard to enforce admission control.
    - Class overload due to lack of centralized ingress.
### QoS Routing
- Routing metrics include QoS requirements.
- Can inform a **source node** of the bandwidth and QoS availability of a destination node and of the path to the destination node
- Challenges:
    - Overhead
    - Route maintenance
    - Mobility responsiveness
    - Guaranteeing reserved resources


# Security
## Symmetric vs Asymmetric Cipher
### Symmetric Cipher
- Fast and secure for privacy/integrity.
- Requires shared key → poor scalability.
### Asymmetric Cipher (Public-Key Encryption)
- **No prior shared key needed.**
- Scalable but computationally expensive.
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
## RSA Public-Key Encryption
- Each user generates public/private key pairs.
- Public key used for encryption, private for decryption.
- Secure if large prime numbers are chosen.
## Key Management in Ad-Hoc Networks
- Challenges: No infrastructure, mobility, physical vulnerability.
#### Building Trust in Ad-Hoc Networks
In ad-hoc networks, where devices don't know each other and there's no central authority, building trust is crucial for managing keys. 
Trust is built through distributed methods:
- **Reputation-Based Trust:** Nodes observe each other's behavior. Honest actions build good reputations, making those nodes more trusted. However, this can be slow to establish and vulnerable to initial "good behavior" by malicious nodes.
- **Pre-existing Knowledge (Bootstrapping):** A small set of initially trusted nodes can introduce and authenticate others, extending a "web of trust."
- **Trust Paths:** If Node A trusts Node B, and Node B trusts Node C, Node A might indirectly trust Node C. The challenge is verifying the strength of these paths in a dynamic network.
- **Out-of-Band Verification:** Sometimes, trust can be established through physical interaction, like scanning a QR code with a public key in person before digital connection.

### SOPKM (Self-Organized Public Key Management)
- Users issue and store certificates locally.
- Trust graph built by users signing each other’s public keys.
- Uses:
    - Certificate exchange via hash IDs.
    - Certificate update before expiry.
    - Revocation by issuer.
    - Conflict resolution by comparing bindings.
- Asks from neighbors for certificates it doesn't have
## SSAWN (Self-Securing Ad-Hoc Wireless Networks)
- No central authority.
- Trust based on endorsements from `k` neighbors.
### Core Ideas
- Shared secret key (SK) is distributed among nodes.
- Each node holds a **partial secret**.
- Certificates are generated via **threshold cryptography**.
- Malicious nodes cannot renew certificates.
- Valid certificates mean global trust.
- Emphasizes local monitoring and collaboration for global trust.

