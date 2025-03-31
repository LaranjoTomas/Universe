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

