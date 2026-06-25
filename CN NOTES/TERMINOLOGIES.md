## 🌐 1. Application Layer Terminologies

- **HTTP (Hypertext Transfer Protocol):** The universal application-layer protocol used by browsers and servers to format, send, and interpret web pages, images, and API data.
    
- **Idempotency:** A property of an HTTP method where making multiple identical requests has the exact same effect on the server state as making a single request (e.g., `GET`, `PUT`, `DELETE` are idempotent; `POST` is not).
    
- **Head-of-Line (HoL) Blocking:** A performance bottleneck where a line of packets is held up by the slow processing or loss of the single packet at the very front of the line.
    
- **Multiplexing:** The ability to send multiple requests and responses concurrently over a single connection line without them blocking each other (introduced in HTTP/2).
    
- **QUIC:** A modern UDP-based transport protocol built by Google that eliminates transport-layer Head-of-Line blocking and combines connection and encryption setups into a single step (used in HTTP/3).
    
- **DNS (Domain Name System):** The distributed hierarchical database ("internet phonebook") that translates human-readable domain names (like `google.com`) into machine-readable IP addresses.
    
- **Recursive Query:** A DNS query where the client demands a definitive answer from the resolver (_"Give me the exact IP or tell me you can't find it"_).
    
- **Iterative Query:** A DNS query where a server responds with the best information it currently has, pointing the resolver to the next authoritative server down the line.
    
- **TTL (Time to Live - DNS):** A value in a DNS record (measured in seconds) that tells caching servers how long they are allowed to store that record before it expires and must be refetched.
    
- **CDN (Content Delivery Network):** A globally distributed network of edge proxy servers that cache static assets (images, videos, JS files) physically close to users to reduce latency.
    
- **Forward Proxy:** An intermediary server that sits in front of a **client** to hide their identity, block certain outbound websites, or cache frequent requests.
    
- **Reverse Proxy:** An intermediary server that sits in front of **backend servers** to protect them, distribute traffic, or handle SSL decryption.
    

## 🛩️ 2. Transport Layer Terminologies

- **Process-to-Process:** The core responsibility of the transport layer—ensuring data moves from a specific application process on your machine to a specific application process on the destination machine.
    
- **Port Number:** A 16-bit numerical identifier used to direct traffic to the correct application on a host (e.g., Port 80 for HTTP, Port 443 for HTTPS).
    
- **TCP (Transmission Control Protocol):** A connection-oriented, reliable, and ordered transport-layer protocol that guarantees data delivery at the expense of speed.
    
- **UDP (User Datagram Protocol):** A connectionless, lightweight, and unreliable transport-layer protocol that prioritizes speed and low latency (fire-and-forget).
    
- **SYN & ACK Flags:** Core control bits in a TCP header. `SYN` (Synchronize) initiates a connection request; `ACK` (Acknowledge) confirms the successful receipt of a packet.
    
- **3-Way Handshake:** The 3-step synchronization process (`SYN` $\rightarrow$ `SYN-ACK` $\rightarrow$ `ACK`) TCP uses to establish a reliable connection before sending data.
    
- **4-Way Teardown:** The 4-step graceful shutdown process used to close a full-duplex TCP connection securely from both sides using `FIN` and `ACK` flags.
    
- **`TIME_WAIT`:** A mandatory TCP state where the active-closing device waits for a duration of $2 \times \text{MSL}$ after sending the final ACK to ensure stray packets clear out and late FIN packets can be acknowledged.
    
- **MSL (Maximum Segment Lifetime):** The maximum estimated time a packet can wander around the internet fabric before being completely discarded by routers.
    
- **Flow Control:** The process of throttling the sender so it does not overwhelm the **receiver's** memory buffer (uses the **Receive Window (`rwnd`)** via the Sliding Window protocol).
    
- **Congestion Control:** The process of throttling the sender so it does not overwhelm the intermediate **routers/network links** (uses the **Congestion Window (`cwnd`)**).
    
- **Slow Start:** The initial phase of TCP congestion control where the sender aggressively doubles its `cwnd` exponentially with every round-trip until it hits a designated threshold (`ssthresh`).
    
- **Congestion Avoidance:** The cautious phase entered after `ssthresh` where the sender increases its `cwnd` linearly (+1 segment per round-trip) to safely test the network limits.
    
- **Fast Retransmit:** An optimization where a TCP sender immediately retransmits a missing packet after receiving **3 duplicate ACKs**, completely bypassing the slow timeout clock.
    

## 🗺️ 3. Network / Internet Layer Terminologies

- **Host-to-Host:** The core responsibility of the Network layer—moving packets across completely separate global networks from the source device to the destination device.
    
- **IPv4 Address:** A 32-bit logical numerical label assigned to a device, typically expressed in dotted-decimal format (e.g., `192.168.1.1`).
    
- **Subnet Mask:** A 32-bit binary mask used by routers to split an IP address into its **Network ID** (the neighborhood) and its **Host ID** (the specific house).
    
- **CIDR (Classless Inter-Domain Routing):** The flexible slash-notation system (e.g., `/24`) that replaced fixed address classes, allowing for variable-sized network allocation.
    
- **Broadcast Address:** The very last IP address in a subnet range (all host bits set to 1) used to blast a packet simultaneously to every single machine on that specific local network.
    
- **NAT (Network Address Translation):** The architectural process of mapping private local IP addresses to a single public IP address at the router level.
    
- **NAPT / IP Masquerading:** A specialized flavor of NAT that allows thousands of local private devices to share a single public IP address concurrently by altering and tracking their **Layer 4 Port Numbers**.
    

## 🔌 4. Data Link & Physical Layer Terminologies

- **Node-to-Node:** The core responsibility of the Data Link layer—moving data reliably between two physically adjacent devices on the _exact same_ local wire segment.
    
- **MAC Address (Media Access Control):** A unique, permanent 48-bit physical hardware identifier burned into a device's Network Interface Card (NIC) at the factory.
    
- **Framing:** The process at Layer 2 of wrapping an IP packet inside a structural frame, attaching a header (MAC addresses) and a trailer (error checking parameters).
    
- **CRC (Cyclic Redundancy Check):** A mathematical checksum appended to a frame's trailer used by the receiving device to instantly detect and drop corrupted data.
    
- **ARP (Address Resolution Protocol):** The local protocol used to dynamically discover a device's physical MAC address when only its logical IP address is known.
    
- **ARP Spoofing / Poisoning:** A Man-in-the-Middle (MITM) network attack where a malicious actor sends fake ARP messages to map their own MAC address to a legitimate local IP (like the router).
    
- **DAI (Dynamic ARP Inspection):** A security feature on enterprise network switches that cross-checks incoming ARP packets against a trusted database to block ARP spoofing attacks.
    

## 📦 5. Systems & Encapsulation Terminologies

- **Encapsulation:** The top-down process where data moves down the stack and each layer wraps it inside a new structural header containing controlling metadata.
    
- **Decapsulation:** The bottom-up process where a receiving machine strips off headers layer-by-layer as the data moves back up the stack to the application.
    
- **PDU (Protocol Data Unit):** The generic term for data at different layers. You must remember the exact naming sequence:
    
    - Application Layer $\rightarrow$ **Data**
        
    - Transport Layer $\rightarrow$ **Segment** (TCP) / **Datagram** (UDP)
        
    - Network/Internet Layer $\rightarrow$ **Packet**
        
    - Data Link Layer $\rightarrow$ **Frame**
        
    - Physical Layer $\rightarrow$ **Bits**