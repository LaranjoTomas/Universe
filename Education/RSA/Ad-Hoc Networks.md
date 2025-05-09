---
tags:
  - "#AD-Hoc"
  - "#Networks"
  - "#RSA"
---
# Mobile Ad-hoc Networks
## Characteristics
- Terminal may appear and disappear anywhere and anytime, and may freely move.
- Nodes can act as routers or terminals
- Networks independently formed, can be merged and splitted anytime
- Dynamic topologies
- Coexistence of different access mediums
- Network is intelligent and self-organized
- Bandwidth constrained, variable capacity link
- Energy contrained operation
- Limited physical security
## Challenges in Mobile Environments
Ad-hoc networks introduce additional challenges beyond traditional networking
### **Limitations of the Wireless Network**
- Lack of central entity for organization
- Limited range of wireless communication.
- Packet loss due to transmission errors.
- Variable capacity links.
- Frequent disconnections and network partitions.
- Limited communication bandwidth.
- Broadcast nature of communications.
### **Limitations Imposed by Mobility**
- Dynamically changing topologies and routes.
- Lack of mobility awareness by system/applications.
### **Limitations of the Mobile Devices**
- Short battery lifetime.
- Limited computational and storage capacities.


# Routing

## Challenges and Requirements
### Major Challenges
- **Mobility** – Path breaks, packet collisions, transient loops.
- **Bandwidth constraint** – Channel shared by all nodes in the broadcast region.
- **Error-prone and shared channel** – Consideration for high bit error rates (BERs) in wireless ad-hoc networks.
- **Location-dependent contention** – High contention when the number of nodes increases.
### Major Requirements
- **Minimum route acquisition delay** – Routes should be discovered quickly.
- **Quick route reconfiguration** – Handle path breaks efficiently.
- **Loop-free routing** – Avoid resource wastage due to routing loops.
- **Distributed routing approach** – Reduce bandwidth consumption.
- **Minimum control overhead** – Optimize bandwidth usage and reduce collisions.
- **Scalability** – Efficient routing for large networks while minimizing control overhead.
- **Provisioning of QoS** – Support for time-sensitive traffic to ensure Quality of Service.
- **Security and privacy** – Ensure resilience against threats and vulnerabilities.

# Proactive and Reactive Protocols in MANETs
## Proactive Protocols
- **Always maintain routes** – Routing tables are updated regularly.
- **Little or no delay for route determination** – Since routes are precomputed.
- **Consume bandwidth** – Periodic updates to keep routes up-to-date.
- **Maintain unused routes** – Routes that may never be used still consume resources.
## Reactive Protocols
- **Lower overhead** – Routes are determined only when needed.
- **Significant delay** – Route discovery process adds latency.
- **Employ flooding** – Uses a global search mechanism to find routes.
- **Bursty control traffic** – Route requests can cause spikes in network traffic.
## Trade-offs
- The best approach depends on **traffic patterns** and **mobility** in the network.
- **Proactive protocols** suit **low-mobility, high-traffic** environments where quick access to routes is required.
- **Reactive protocols** are better for **high-mobility, sporadic traffic** scenarios to reduce unnecessary overhead.

## Reactive routing protocols
### AODV - Ad-Hoc On-Demand Distance Vector Routing
#### Overview
- AODV maintains **routing tables** at nodes, eliminating the need for packets to store routes.
- Routes are established **only when needed**, reducing overhead.
#### AODV Operation
##### Route Request (RREQ)
- When a node **re-broadcasts** a **Route Request (RREQ)**, it sets up a **reverse path** to the source.
- AODV assumes **bi-directional links**.
- When the **destination** receives the RREQ, it replies with a **Route Reply (RREP)**.
- The RREP **travels along the reverse path** set up by RREQ forwarding.
##### Sequence Numbers
- Each node maintains **non-decreasing sequence numbers** for route freshness.
- Sequence numbers are included in **RREQ** and **RREP** messages.
- Intermediate nodes can reply with RREP **if they have a fresher route** than the sender.
- Routing table entries have a **lifetime** and are deleted upon expiration.
#### Route Request and Route Reply
- **RREQ** includes the last known **sequence number** for the destination.
- **Intermediate nodes** may reply with an RREP if they have a more recent route.
- Forwarding an RREP causes nodes to record the **next hop** to the destination.
- Unused **reverse path entries** are purged after a timeout.
- **Forward path entries** are removed if unused for an `active_route_timeout` interval.
#### Link Failure & Route Errors
- Nodes **exchange hello messages** periodically to maintain active links.
- If a **link breaks**, all active neighbors are notified.
- **Route Error (RERR) messages** propagate link failures and update destination sequence numbers.
##### Route Error (RERR)
- If a packet cannot be forwarded, the node sends an **RERR** message.
- The node **increments the destination sequence number** and includes it in RERR.
- When the source node **receives an RERR**, it initiates a **new route discovery**.
##### Local RERR (Local Repair)
- Triggered by link-layer acknowledgments or hello messages.
- Detecting node may attempt **local repair** by sending an RREQ.
- RERR is **sent to precursors** (nodes that recently forwarded a packet via the broken link).
- **Recursively propagated** to inform affected nodes.
#### AODV Summary
- Nodes maintain **routing tables** only for **active** routes.
- **Single next-hop** per destination is maintained at each node.
- **Sequence numbers** prevent stale/broken routes.
- **Unused routes expire** even if topology remains unchanged.

### LAR - 


## Proactive Routing Protocols 

### OLSR - Optimized Link State Routing Protocol 
#### Overview
- **OLSR** is a proactive routing protocol.
- Uses an **efficient link state packet forwarding mechanism**.
- Designed to reduce control overhead in mobile ad-hoc networks.
#### Multipoint Relaying (MPR)
- **Multipoint Relays (MPRs)** are selected neighbors that forward link state packets.
- **Key Goals:**
  - Reduce the size of control packets.
  - Limit forwarding to only a **subset of nodes**.
- **Link State Forwarding:**
  - For example, if nodes **C** and **E** are MPRs for node **A**, they forward link state information from A.
  - Similarly, nodes **E** and **K** may be MPRs for node **H**, with node K forwarding H’s information.
#### MPR Sets and MPR Selectors
- **MPR Sets:**
  - The set of nodes that a given node selects as MPRs.
  - Only these nodes **process and forward** the originating node’s link state packets.
- **MPR Selectors:**
  - The set of neighbors that have selected a given node as their MPR.
  - A node forwards packets received from its MPR selectors.
- **Dynamic Nature:**
  - Both MPR sets and selectors can change over time based on network conditions and efficient selection algorithms.
#### MPR Selection Criteria
- **Selection Steps:**
  1. **Bidirectional Links:** Choose every node in the two-hop neighborhood that has a bidirectional link.
  2. **Cover Isolated Nodes:** Ensure nodes with only a single path (isolated nodes) are covered.
  3. **Maximal Coverage:** Select nodes that cover the maximal number of two-hop neighbors.
#### Neighbor Relationships
- **Hello Messages:**
  - Each node periodically sends a “Hello” message.
  - Purpose: **Advertise its presence**, discover neighbors, and assist in MPR selection.
#### MultiPoint Relays (MPR) Role
- **Primary Function:**
  - Act as routers by passing topology control messages.
  - **Minimize retransmissions** to reduce overhead.
- **Routing Backbone:**
  - MPRs form a **routing backbone**; other nodes act as hosts.
#### Structure of an OLSR Network
- **MPR Backbone:**
  - MPRs build the backbone of the network.
  - **Host nodes** (non-MPRs) rely on this backbone for routing.
- **Dynamic Topology:**
  - As nodes move, the **topological relationships and routes change**.
  - The shape and composition of the MPR backbone **evolve** with the network.
### B.A.T.M.A.N
#### Overview
- **Traditional Routing:**  
  - Nodes exchange control packets containing link state information (e.g., utilization, bandwidth).
  - Each node must have near-exhaustive knowledge of the network.
- **BATMAN Approach:**  
  - Instead of detailed link state data, BATMAN uses the **presence or absence** of control packets (OGMs) to indicate link and path quality.
  - This enables nodes to determine the best next-hop without full network knowledge.
#### BATMAN Operation
##### Originator Messages (OGMs)
- **Periodic Transmission:**  
  - Every node sends an OGM periodically, which includes a **sequence number**.
- **Multiple Paths:**  
  - For instance, if Node A has two neighbors (B and C) and receives Node D's OGMs via both, it can choose either as the next hop.
- **Link Failure Impact:**  
  - If a link (e.g., between A and C) goes down, OGMs from D will only be received via B, making B the preferred next hop for reaching D (and even for reaching C).
##### Sliding Window Mechanism
- **Purpose:**  
  - To track the reception of OGMs over a set of sequence numbers (e.g., WINDOW_SIZE = 8).
- **Operation:**  
  - A sliding window indicates which sequence numbers have been received.
  - When an out-of-range sequence (e.g., seq# 17) is received, the window shifts accordingly, updating the count of in-range sequences.
##### Routing Table & Next-Hop Determination
- Each node maintains a **sliding window for every originator** (other node) for each neighbor.
- BATMAN infers **link and path quality** from the successful (or missing) retransmission of OGMs.
- **Collective Intelligence:**  
  - The retransmission behavior indicates which neighbor provides the best link quality.
- **Routing Information:**  
  - Nodes only need to know the **next hop** to a destination, derived from the OGMs, rather than full network topology.
#### Transmission Quality (TQ) Metrics
##### BATMAN v.4: Basic TQ Calculation
- **Formula:**  
  - `TQ = TQ_incoming * TQ_local`
- **Process:**  
  - A node broadcasts an OGM with maximum TQ.
  - The receiving node applies the TQ calculation and rebroadcasts it.
  - Subsequent nodes can infer the quality of the link toward the originator.
##### BATMAN v.5: Enhanced Throughput-Based Metrics
- **Why Throughput?**  
  - Packet loss alone is insufficient, especially with many modern devices and low-loss links.
- **Automatic Throughput Estimation:**  
  - **Wireless:** Modern WiFi drivers provide estimated throughput per neighbor.
  - **Wired:** Devices can export theoretical throughput via APIs like `ethtool`.
  - If direct querying fails, BATMAN V initiates periodic throughput tests.
- **Path Throughput:**  
  - Calculated as the **minimum throughput** across all links in the path between two nodes.
#### Summary
- **BATMAN minimizes control overhead** by using OGMs and sliding windows instead of exhaustive link state information.
- **Link quality is inferred** from the presence, absence, and sequence continuity of OGMs.
- **Routing decisions are made locally** based on the best next-hop as determined by OGM propagation.
- **Enhanced metrics (throughput)** in BATMAN v.5 further refine route quality estimation to support dynamic and heterogeneous networks.
