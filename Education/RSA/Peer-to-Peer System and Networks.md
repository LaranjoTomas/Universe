---
tags:
  - "#Networks"
  - "#RSA"
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