---
tags:
  - "#Networks"
  - "#RSA"
  - "#CyberSecurity"
  - "#Security"
---
### Content Distribution Networks (CDN)

When a client requests access to an application’s main server, they are redirected to a nearby replica site. These distributed sites cache content and direct the client to the closest available location, reducing congestion and improving performance.  

The primary goal of a CDN is to replicate content across the internet, ensuring availability and optimizing access speed.  

**Key Components:**  
- **Distributed servers** that store cached content.  
- **A high-speed network** connecting these servers.  
- **Clients** from anywhere in the world seeking fast web performance.  
- **A binding mechanism** that efficiently directs clients to the best server.  

<img src="CDN_Architecture.png" style="display: block; margin: auto;" />

#### CDN Infrastructure

- **Content Delivery Infrastructure**: Delivers content to clients from surrogate servers.  
- **Request Routing Infrastructure**: Directs client requests to the most suitable surrogate.  
#### CDN Components:  
- **Distribution Infrastructure**: Moves or replicates content from the origin server to surrogates, using methods like DNS redirection and anycast.  
- **Accounting Infrastructure**: Logs and reports distribution and delivery activities.  

<img src="CDN_Infrastruct.png" style="display: block; margin: auto;" />

### Peer-to-Peer networks

P2P (network architecture) networks  leverage diverse connectivity and the combined bandwidth of participants. They are commonly used for large-scale connections, such as file sharing (audio, video, data) and even real-time communication (e.g., Skype).

**Pure P2P Networks:**
There are no dedicated clients or servers, all nodes function equally as both clients and servers, communicating directly and sharing resources.

#### Peer-to-Peer Model

Each peer has similar resources, enabling direct communication and efficient resouce sharing. Common P2P services and applications include:
- **Distributed Computing**
- **File Sharing**
- **Collaboration**

- **VoIP**
- **Streaming Media**
- **Instant messaging**
- **Software publication and distribution**

#### Types of P2P Networks

- **Pure P2P**: Refers to an environment where all participating nodes are peers. No central system controls, coordinates, nor facilitates the exchanges among peers.
  
- **Hybrid P2P**: Refers to an environment where there are servers to enable peers to interact with each other. The degree of central system involvement varies depending on the application. Different peers may have different functions (e.g., simple nodes, routers, rendezvous points). Hybrid systems are often used depending on the specific needs of the application.

#### Advantages of Peer-to-Peer

Clients can contribute **bandwidth, storage, and computing power**. The system scales dynamically as new nodes join, increasing total capacity. A **distributed structure** imprives robustness, as data is replicated across multiple peers, reducing reliance on a centralized index server.

#### Challenges of Peer-to-Peer

- **Peer discovery and group management**: Finding and managing peers in a decentralized environment can be complex, especially as the network grows.
- **Data location, searching, and placement**: Efficiently locating data and determining its placement within the network can be challenging without a centralized directory.
- **Search and routing**: Algorithms for searching and routing requests efficiently across the network are critical to performance.
- **Reliable and efficient file delivery**: Ensuring that files are delivered reliably and efficiently, even in a highly distributed environment, remains a key challenge.
- **Security/privacy/anonymity/trust**: Since P2P networks involve direct interaction between users, ensuring security, maintaining privacy, protecting anonymity, and establishing trust among peers are major concerns.

#### Peer-to-Peer Node Types

- **Simple Peers**:
  - A simple peer represents a single end-user, providing services from their device and consuming services from other peers in the network.
  - Typically located behind a firewall, making direct communication with peers outside the firewall challenging.
  - Due to limited network accessibility, simple peers have the least responsibility in a P2P network and are not responsible for handling communication on behalf of other peers.

- **Rendezvous Peers**:
  - Act as gathering or meeting places for peers in the network to discover each other and their resources.
  - Peers issue discovery queries to a rendezvous peer, which responds with information about other peers it is aware of.
  - Can cache peer information or forward discovery requests to other rendez-vous peers, improving responsiveness and reducing network traffic.
  - Typically located outside a private internal network’s firewall, but they may need to traverse firewalls or be located behind one with the proper configuration.
<img src="rendezvous_peers.png" style="display: block; margin: auto;" />

- **Router (Relay) Peers**:
  - Provide mechanisms for peers to communicate across firewalls or NAT (Network Address Translation) equipment.
  - Enable peers behind firewalls to communicate with peers outside the firewall and vice versa.
  - A relay peer is part of the data stream, while a rendez-vous peer is part of the discovery path.

#### Structured vs Unstructured Peer-to-Peer Networks

- **Unstructured P2P Networks**:
  - The network overlay links are formed arbitrarily.
  - To find a piece of data, queris are fooded across the network to as many peers as possible that might share the data.
  - The queries may not always be resolved, especially for rare data, which is less likely to be found.
  - Flooding leads to high signaling traffic.
  - Examples: Gnutella, FastTrack/KaZaa, BitTorrent.

- **Structured P2P Networks**:
  - These networks use a globally consistent protocol that allows efficient routing of searches to peers that have the desired file, even if it is rare.
  - A common implementation is the **Distributed Hash Table (DHT)**, using consistent hashing to assign file ownership to specific peers.
  - Examples: Chord, Pastry, Tapestry, CAN, Tulip, Kadmelia, BitTorrent (trackerless), IPFS.

### Fully Decentralized Information Systems

#### P2P File Sharing
- A global-scale application of peer-to-peer networks.
- Example: **Gnutella**
  - 40,000 nodes, 3 million files (August 2000)
  - 3 million nodes (January 2006)
- **Gnutella has no central servers.**
#### Strengths
- Good response time, **scalable**.
- No **infrastructure** or **administration** required.
- No **single point of failure**.
#### Weaknesses
- Generates **high network traffic**.
- No **structured search** mechanism.
- Susceptible to **free-riding** (peers consuming resources without contributing).
#### Gnutella: Meeting Peers (Ping/Pong)

<img src="gnutella_example.png" style="display: block; margin: auto;" />

Gnutella relies on a **Ping/Pong mechanism** to discover peers:
- **Ping**: Announces a peer's availability and probes for other servents.
- **Pong**: Response to a ping, providing details about the responding peer.

<img src="gnutella_response_example.png" style="display: block; margin: auto;" />

#### Gnutella: Protocol Message Types


| Type     | Description                                           | Contained Information                                                            |
| -------- | ----------------------------------------------------- | -------------------------------------------------------------------------------- |
| Ping     | Announce availability and probe for other servents    | None                                                                             |
| Pong     | Response to a ping                                    | IP address and port# of responding servent; number and total kb of files shared  |
| Query    | Search request                                        | Minimum network bandwidth of responding servent; search criteria                 |
| QueryHit | Returned by servents that have the requested file     | IP address, port# and network number of results and result set                   |
| Push     | File download requests for servents behind a firewall | Servent identifier; index of requested file; IP address and port to send file to |

#### Searching in Gnutella (Structureless)

- Queries are **flooded** to neighboring peers.
- Queries have a **Time-To-Live (TTL)** and are **forwarded only once**.
- A query may receive multiple responses from peers providing the requested file.
- The requester selects one peer and **directly contacts** it to download the file.

<img src="gnutella_structureless.png" style="display: block; margin: auto;" />

#### Improvements of Message Flooding

To reduce network traffic and improve efficiency, Gnutella introduced search optimizations:
##### **Expanding Ring**
- Start with a **small TTL** (e.g., `TTL = 1`).
- If no results are found, **increase TTL iteratively** (e.g., `TTL = TTL + 2`).
##### **k-Random Walkers**
- Forward the query to **one randomly chosen neighbor** with a **large TTL**.
- Start **k random walkers** to explore different parts of the network.
- Random walkers **periodically check with the requester** to determine whether to continue searching.

#### Hybrid Gnutella: "Ultrapeers"

- **Ultrapeers** can be **installed** or **self-promoted**.
- Ultrapeers act as intermediaries to **reduce query traffic for leaf nodes**.

#### Real Gnutella Network Examples

- **Popular open-source file-sharing network**.
  - ~450,000 users as of **2003**.
  - ~2,000,000 users as of **2022**.

- **Ultrapeer-Based Topology**
 - Queries are **flooded among ultrapeers**.
 - **Leaf nodes** are **shielded** from excessive query traffic.
 - The network structure is **mapped using multiple crawlers**.

<img src="gnutella.png" style="display: block; margin: auto;" />

### OpenNap/Napster

- **Files** are stored **on client machines**.
- **Servers** provide **search functionality** (rendezvous role) and initiate **direct transfers** between clients.
- **OpenNAP** is an extension that allows linking multiple servers.
- **Network Architecture**: **Hybrid Unstructured**.
- **Algorithm**: **Centralized Directory Model (CDM)**.
<img src="OpenNAP.png" style="display: block; margin: auto;" />
### FastTrack / KaZaA
- An **extension of the Gnutella protocol** with added **super-nodes** for improved scalability (similar to **Gnutella v.2**).
- A **powerful peer** with a **fast connection** automatically becomes a **super-node**, acting as a **temporary indexing server** for slower peers.
- Super-nodes **communicate with each other** to process search requests efficiently.
- **Network Architecture**: **Hybrid Unstructured**.
- **Algorithm**: **Flooded Requests Model (FRM)**.
<img src="FastTrack.png" style="display: block; margin: auto;" />


### BitTorrent

- BitTorrent **offloads file tracking** to a central **tracker** server.
- Uses a principle called **tit-for-tat**:
  - To **receive** files, you must **share** files.
  - **Prevents leeching** and encourages fair sharing.
- Enables **fast downloading** of large files while using **minimal bandwidth**.
- **Network Architecture**: Hybrid Unstructured.
- **Algorithm**: Centralized Directory Model (CDM).
#### **Key Terminology**
- **`.torrent` file**: A **pointer file** that directs the computer to the file it wants to download.
- **Swarm**: A group of computers **simultaneously downloading** or **uploading** the same file.
- **Tracker**: A **server** that manages the BitTorrent file transfer process.
#### How BitTorrent works
1. **BitTorrent client** software communicates with a **tracker** to find:
   - **Seeders**: Computers with the **complete file**.
   - **Peers (Leechers)**: Computers currently **downloading** the file.
2. The **tracker identifies the swarm** and helps the client **trade pieces** of the file with other computers.
3. The computer **receives multiple pieces of the file simultaneously** from different peers.
4. If the user **keeps the BitTorrent client running after download**, others can download the file from them.
   - These users are **ranked higher** in the **tit-for-tat** system.
#### BitTorrent Trackers
- **Trackers monitor** the number of **seeds/peers** and help **downloaders find each other**.
- A **downloader sends status info** to the tracker, which responds with a list of peers **downloading the same file**.
- **Web servers do not store file content**, only **metadata files** describing:
  - **File length, name, etc.**
  - **Tracker URL** associated with the file.
#### Trackerless Torrents & DHT
- Instead of a **central tracker**, some torrents use a **trackerless system**.
- This is achieved through **DHT (Distributed Hash Tables)**, also known as the **"distributed database"**.

<img src="BitTorrent.png" style="display: block; margin: auto;" />

### InterPlantetary File System (IPFS)

A **Global distributed file system** focused on **decentralization** and **content distriution**. The files are in **distributed storages**, with **Distributed Hash Tables** that use hash of files as  the krey to return the location of the file. Once the location is determined, the transfer takes place **peer-to-peer** as a **decentralized transfer**.

#### **Key Features**

- **Content-based identification**
  - Uses a **secure hash** of file contents instead of location-based addressing.
- **Decentralized Storage & Retrival**
  - **Distributed hash Table (DHT)** is used to resolve file locations.
- **Efficient Block Exchange**  
  - Uses **BitTorrent**-like peer-to-peer file distribution.  
- **Incentivized Block Exchange**  
  - Utilizes the **Bitswap protocol** for fair sharing of data.  

- **Versioned File Organization**  
  - **Merkle DAG (Directed Acyclic Graph)** structure, similar to **Git version control**.  
- **Security**  
  - **Self-certifying storage nodes** ensure data integrity and authenticity.  

#### Block Exchange Mechanism

**Bitswap Protocol** incentivizes peer nodes to exchange data blocks. Each peer node maintains:
- **want_list**: Blocks it wants.
- **have_list**: Blocks it possesses.
Any **imbalance** is noted in the form of a **BitSwap credit & Debt**. The nodes in the network must **provide value** by sharing blocks, if a node **sends a block**, it earns an **IPFS token** that can be used later to  **request a block**.
Any issue that may arise is managed by the **Bitswap Protocol**, such as:
- **Freeloading nodes** (taking but not giving),
- **Nodes with nothing to request**,
- **Nodes with no blocks to offer**.

#### Bitswap Calculation

##### **Debt Ration & Probability of Seding Blocks**

**Debt Ratio (r)** determines how much a peer owes. 
$$\Large r = \frac{\text{bytes\_sent}}{\text{bytes\_recv} + 1} $$

The probability of sending a block to a **debtor node** drops **quickly** once its **debt ratio exceeds twice** the established credit.  
$$\Large P(send \mid r) = 1 - \frac{1}{1 + exp(6 - 3r)} $$

##### **BitSwap Ledger System**

Nodes maintain **ledgers** that track exchanges with other nodes. When a **connections is activated**, nodes **exchange ledger information**. If the **ledger mismatches**, it is **reset**, causing the node to **lose its accumulated credit or debt**.

##### **Sketch of a Peer Connection Lifecycle**

1. **Open** – Peers exchange ledgers until they agree.  
2. **Sending** – Peers exchange **want_lists** and **blocks**.  
3. **Close** – Peers deactivate the connection.  
4. **Ignored** – A peer is ignored (timed out) if a node's debt is too high. 

```c
// Protocol interface:
interface Peer {
	open (nodeid : NodeId, ledger :Ledger);
	send_want_list (want_list :WantList);
	send_block (block :Block) -> (complete :Bool);
	close (final :Bool);
}
```

##### Example

1. **Requesting a Block**  
   - A peer **broadcasts a "Want"** message to all **connected peers**.  

2. **Fallback to DHT**  
   - If **no peers respond**, the peer queries the **DHT** to find out **who has the root CID**.  

3. **Session-Based Requests**  
   - Peers that **respond** are **added to a session**.  
   - Subsequent **requests** are sent **only to peers in the session**, improving efficiency.  
<img src="Bitswap.png" width="1200" style="display: block; margin: auto;" />

#### IPFS: Split Factor

When a local node **receives a block**, it **broadcasts a cancel message** for that **CID** to all connected peers. However, **delays in processing the cancel message** can cause **duplicate block transmission**. 
The local node **tracks the ratio** of **duplicate blocks** to **received blocks**. The **Split factor** determines how many peers receive the same CID request. 
If the **duplicate ratio > 4**, the split factor **increases** -> fewer peers receibe the same CID request. 
If the **duplicate ratio < 2**, the split factor **decreases** -> more peers receibe the same CID request.

#### IPFS: Cluster

**Faciliates content replication** across multiple nodes. All cluster peers must share the **same cluster secret** to join, each cluster peer has a **unique ID**, when **new data is added and pinned**  to one peer, **all other peers receive the data**.  The **peer that initiates the cluster** generates the **cluster secret** and becomes the **cluster leader**. Any peer in the cluster can **modify, add, or remove data** and the cluster remains functional even if **peers are added or removed**. If the **leader node goes down**, a new eader is elected using the **RAFT consensus algorithm**.

#### RAFT Consensus Algorithm

1. **Leader Election Process**  
   - A server **becomes a candidate** if it receives no communication from the leader within the **election timeout** (≈ 300ms).  
   - The candidate **increases its term counter, votes for itself, and requests votes** from other servers.  
   - A server **votes only once per term** (first-come-first-served).  
   - If a candidate **receives a majority of votes**, it **becomes the new leader**.  
   - If another server sends a **higher term number**, the candidate **recognizes it as the new leader** and becomes a follower.  

2. **Handling Election Failures**  
   - If no leader is elected (e.g., due to a split vote), a **new election term begins**.  

This **ensures high availability and decentralized coordination** within the **IPFS cluster**.


<img src="RAFT_algo.png" style="display: block; margin: auto;" />

#### IPFS: Locating Nodes & Objects

##### **Locating Nodes**
Nodes are identifies by **cryptographic hashes** of their **public keys**. They **store objects** that make up the files being exchanged, while each **object is identifies by a secure hash**, and objects can contain **sub-objects**, the **root hash** is generated using the **hashes of sub-objects**.
##### **Locating Objects**  
- IPFS uses **content-based identification** instead of location-based addressing (like HTTP).  
- Resources are identified by **hashes of their content**.  
- **Resolving object location:**
  - A request is sent across the network **searching for peers** that hold the resource with the specific hash.  
  - The **DHT (Distributed Hash Table)** in IPFS is used for **node discovery** and **file location resolution**.  
  - The DHT holds:
    - **Key:** Hash of the object.  
    - **Value:** Location(s) of the object.  
  - The key **directly maps to the location** where the file is stored.  

#### IPFS: Objects 

##### **Object pinning**
Nodes can **pin objects** to ensure their **long term availability**. Pinned objects are stored **permantly in the node's local storage**.
##### **Object Publishing**
- IPFS allows **pin distributed publising** via **DHT and content-hash addressing**. 
- Any user can **publish an object** by:
  - Adding its **hash key** to the DHT,
  - Adding themselves as a **peer storing the object**,
  - Sharing the **object path** with other users.
- **Versioning:**
  - **New versions** of files generate **new hashes**, making them **new objects**,
  - **Tracking versions** requires additional **versioning objects**.

This ensures **decentralized, efficient, and resilient file storage and retrieveal**.


### Distributed Hash Tables (DHTs)

#### Searching in DHTs (structured)

- **Exact filename required** for search.  
- **Keys (filenames) map to node IDs** in the network.  
- **Changing a filename** results in a **new hash** → **searching in a different node**.  
<img src="DHT_Structured.png" style="display: block; margin: auto;" />

#### **CHORD: DHT Algorithm**  

##### **Overview**  
- **Every file or data item** in the network has a **unique identifier**.  
- This identifier is **hashed** to generate a **key** for the resource.  
- If a node needs a file, it:  
  - **Hashes** the filename.  
  - **Sends a request** using this key.  

##### **Node Organization in CHORD**  
- All **nodes (peers) also hash their IP addresses**.  
- Nodes **form a ring** in **ascending order** of their hashed IP values.  
- <img src="CHORD.png" style="display: block; margin: auto;" />
##### **Key Lookup & Storage**  
- **Successor Node of a Key (k):**  
  - The **first node whose ID is ≥ k** (or follows k in the ring).  
  - **Denoted as:** `successor(k)`.  
- **Every key (file hash) is assigned to its successor node**.  
- **Looking up a key k = Querying successor(k)**.  

##### **File Sharing in CHORD**  
- A node **wants to share a file**:  
  1. **Hashes** the file identifier → generates key `k`.  
  2. **Sends file identifier + its IP** to `successor(k)`.  
  3. `successor(k)` **stores the information** in the DHT.  

##### **DHT Indexing & Redundancy**  
- **All resources are indexed** across the **entire DHT**.  
- **If multiple nodes hold the same file**, their keys **map to the same DHT entry**.  
- The requesting node can:  
  - Choose **which node to download from**.  
  - Request the file from **multiple nodes for redundancy & speed**.  

#### DHT: Search Information 
##### **Overview**
In a Distributed Hash Table (DHT), nodes store and retrieve data using a **hashing mechanism**. Each node in the network is responsible for a subset of keys. When a node wants to find content:
1. It **hashes the data identifier** and sends a request to `successor(k)`.
2. The system returns the **IP address of the node** that holds the actual data.
3. However, a node might not know the IP of `successor(k)`, only its key.

To solve this problem, **each node maintains a Finger Table**, which enables efficient lookups.
##### **Finger Table Structure**
Every node stores a **Finger Table**, which helps it **route queries faster**. Instead of storing all node addresses, it **stores an exponential sequence of node addresses**.

- Each node holds **entries for nodes that are at distances of powers of 2** from itself.
- **Entry i of node k’s Finger Table holds the successor node at `k + 2^i`**.

###### **Example Finger Table**
For a **node N8**, the finger table might look like this:

| Index (i) | Node + `2^i` | Successor Node |
| --------- | ------------ | -------------- |
| 1         | 8 + 1 = 9    | **N14**        |
| 2         | 8 + 2 = 10   | **N14**        |
| 3         | 8 + 4 = 12   | **N14**        |
| 4         | 8 + 8 = 16   | **N21**        |
| 5         | 8 + 16 = 24  | **N32**        |
| 6         | 8 + 32 = 40  | **N42**        |

Nodes in the table **act as shortcuts**, allowing efficient routing.
<img src="Finger_table.png" style="display: block; margin: auto;" />
#### File Search: Flooding vs. DHTs 

##### **Flooding-Based Search**
- **Pros:**
  - Can quickly discover **popular files**.
  - Handles **arbitrary single-site logic** well.
- **Cons:**
  - **Inefficient for rare items**—may not find them at all.
  - **Consumes a lot of bandwidth (BW)** due to network-wide message propagation.

##### **DHT-Based Search**
- **Pros:**
  - **Guaranteed retrieval** (should never miss a file if it exists).
  - Efficient for **structured queries** (equijoins, selections, aggregates).
- **Cons:**
  - **Cannot perform wildcard searches easily**.
  - **Expensive to publish documents** with many keywords.
  - **Expensive for long intersection queries** (e.g., searching multiple rare terms).

####  **Hybrid Solution: Best of Both Worlds**
A **hybrid search** combines **Flooding** and **DHTs** to leverage their strengths:
- **Popular files** → Searched using a **flood-based** approach.
- **Rare files** → Searched using a **DHT-based** approach.
- The **system intelligently decides** which method to use based on query type and expected results.
<img src="Hybrid_Search.png" style="display: block; margin: auto;" />
The image illustrates how **Hybrid Search** works by using **both Flood-based and DHT-based searches**:
1. **Flood-Based Network (Left Cloud)**
   - Used to **search for popular items**.
   - Returns **more results quickly**.
   - Uses a **flood query** mechanism.

1. **DHT Network (Right Cloud)**
   - Used to **search for rare items**.
   - May return **very few results**.
   - Uses a **DHT query** mechanism.

3. **Computer (Bottom Center)**
   - Sends either a **Flood Query** (for popular files) or a **DHT Query** (for rare files).
   - The goal is to **balance efficiency and search accuracy**.

### Security

#### Security Measures

Most attacks can be mitigated through **careful network design** and **encryption**. However, no system is completely secure if a **majority of peers are malicious**.
##### Anonymity
Some P2P networks (e.g., **Freenet**) hide user identities by **passing traffic through intermediate nodes**.

##### Encryption
- Encrypts **peer-to-peer traffic flows** to:
  - **Prevent ISPs from detecting** and throttling/blocking P2P usage.
  - **Hide file contents** from eavesdroppers.
  - **Complicate law enforcement & censorship** of certain materials.
  - **Authenticate users** and prevent "man-in-the-middle" attacks.
  - **Maintain anonymity** for users.
##### How to Mitigate These Attacks?
- **Use encryption** (e.g., **end-to-end encryption, VPNs, Tor routing**).
- **Verify file integrity** (e.g., **hash checking, reputation-based downloads**).
- **Avoid suspicious software** (e.g., **use open-source clients**).
- **Implement authentication** to **prevent identity spoofing**.
- **Monitor network activity** for unusual behavior.
