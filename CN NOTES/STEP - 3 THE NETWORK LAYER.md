The Transport Layer manages data between applications, but it relies completely on the **Network Layer** to handle host-to-host communication—actually getting packets from one physical machine to another across the globe

### 1. Understanding the IPv4 Address Structure

An IPv4 address is a **32-bit numerical address** written in dotted-decimal format (four 8-bit octets separated by dots, like `192.168.1.50`).

An IP address is never just a random number. It is always divided into two structural components:

1. **Network ID:** Identifies the specific network or "neighborhood" the device lives in.
    
2. **Host ID:** Identifies the specific device or "house" within that neighborhood.
    
How do routers know where the Network ID ends and the Host ID begins? They use a **Subnet Mask**.

### 2. What is Subnetting and a Subnet Mask?

A subnet mask looks just like an IP address (e.g., `255.255.255.0`), but its bits are continuous `1`s followed by continuous `0`s.

- The `1`s mask out the **Network** portion.
    
- The `0`s leave room for the **Host** portion.
    

If a router takes an IP address and performs a bitwise `AND` operation with its subnet mask, the result tells the router exactly what network that packet belongs to.
#### Why do we subnet?

If the internet were just one giant pool of 4.3 billion addresses, routers would have to maintain a table tracking every single device on Earth. That's impossible. By breaking a large network into smaller sub-networks (subnets), we isolate local traffic and make routing incredibly efficient.

### 3. CIDR (Classless Inter-Domain Routing) Notation

Historically, the internet used rigid "Classes" (Class A, B, C) which wasted millions of addresses. If you needed 300 addresses, a Class C (254 hosts) was too small, so you had to buy a Class B block (65,536 hosts), wasting 65,000+ addresses.

To fix this, **CIDR** was introduced. It discards fixed classes and allows flexible network boundaries using a **Slash (`/`) Notation**, which tells you exactly how many bits belong to the network.

Let's look at a classic interview math example:

> **Interview Problem:** Given the IP address `192.168.1.0/24`, calculate the total number of usable host addresses.

1. **Understand the slash:** `/24` means the first 24 bits are fixed for the Network ID.
    
2. **Calculate remaining bits:** A total IPv4 address has 32 bits. $32 - 24 = 8 \text{ bits}$ are left over for hosts.
    
3. **Calculate total combinations:** $2^8 = 256$ total addresses.
    
4. **Subtract the 2 reserved addresses:** $256 - 2 = \mathbf{254 \text{ usable hosts}}$.
    

####  Why do we subtract 2?

Every single subnet in existence reserves two specific addresses that can never be assigned to a real device:

- **The First Address (`.0` in a `/24`):** The **Network Address** itself (used to identify the network in routing tables).
    
- **The Last Address (`.255` in a `/24`):** The **Broadcast Address** (sending a packet to this address blasts it to every single machine on that specific subnet).

#### Imagine you look up your IP address at home or college, and it says `192.168.1.50`. That is a **Private IP address**. It works perfectly inside your local building, but it is completely unroutable on the public internet. Furthermore, if there are 500 students in your university or 5 devices in your home, how can all of them browse the internet simultaneously if your internet provider only assigns you **one single public IP address**?

## Step 3, Topic B: NAT (Network Address Translation)

In the previous topic, we calculated that a `/24` network has only 254 usable addresses. Globally, IPv4 provides roughly 4.3 billion addresses. With billions of smartphones, smart TVs, laptops, and servers online, the math doesn't add up—we should have completely run out of addresses years ago.

The reason the internet still works today is **NAT (Network Address Translation)**.

### 1. Public vs. Private IP Addresses

To make NAT work, the internet architecture splits the IPv4 universe into two distinct worlds:

- **Private IP Addresses:** Used inside local networks (your home, office, or university campus). These addresses can be duplicated globally. Your home router might assign your laptop `192.168.1.5`, and your neighbor's router might assign their laptop the exact same `192.168.1.5`. These are **unroutable** on the public internet.
    
- **Public IP Addresses:** Globally unique addresses assigned to internet-facing devices (like your router's external interface or Google's servers). No two devices on the public internet can share a public IP.
    

### 2. How NAT Works (The Airport Analogy)

Think of your local network as an office building where every desk has an internal extension number (Private IP). The building only has **one primary telephone number** (Public IP) handled by the receptionist (The Router).

When you want to call someone outside the building, the receptionist establishes the line, masks your extension, and uses the main corporate number to place the call.

#### The Step-by-Step Packet Flow:

1. **The Request:** Your laptop (`192.168.1.5`) wants to fetch a webpage from a server (`140.82.121.3`). It creates a packet:
    
    - **Source IP:** `192.168.1.5` | **Source Port:** `50001`
        
    - **Destination IP:** `140.82.121.3` | **Destination Port:** `443`
        
2. **The Translation:** The packet hits your home router. The router realizes a private IP cannot go onto the internet. It swaps your private IP out for its own unique **Public IP** (`203.0.113.1`).
    
3. **The NAT Table Entry:** To ensure it knows where to send the data when the server replies, the router records this mapping in its internal **NAT Translation Table**:
    

|**Private LAN IP & Port**|**Public WAN IP & Assigned Port**|**Destination Server IP & Port**|
|---|---|---|
|`192.168.1.5:50001`|`203.0.113.1:62000`|`140.82.121.3:443`|

4. **The Delivery & Return:** The packet travels across the internet. The destination server sees the source as `203.0.113.1:62000`. When the server replies, it sends the data back to that public address.
    
5. **The Reverse Translation:** Your router receives the incoming packet on port `62000`. It checks its NAT table, finds the match, swaps the public destination IP back to your private IP (`192.168.1.5:50001`), and forwards it to your laptop.

## What is NAPT / Masquerading?

NAPT is an extension of NAT that allows **thousands of devices on a private local network to share a single public IP address simultaneously**.

It achieves this by translating not just the IP addresses, but also the **Layer 4 Port Numbers** of the transport layer (TCP/UDP). Because the router uses ports to distinguish between different devices, this process is called _masquerading_—all internal devices hide behind the exact same public IP identity.

### 1. The Math: How many devices can share one IP?

A standard TCP/UDP port field is **16 bits** long.

- This means there are $2^{16} = 65,536$ available ports.
    
- Excluding well-known system ports (0–1023), a router using NAPT can theoretically track roughly **64,000 concurrent connections** using just a single public IP address.
### 2. Security Side-Effect (The Accidental Firewall)

NAPT provides an inherent layer of security to local area networks. Because a public device can only get through the router if there is an _active, pre-existing entry_ inside the NAPT table, **no external device on the internet can initiate a connection to your laptop directly**.

If a hacker tries to send malicious packets to `203.0.113.1:65000`, the router looks at its table, sees no internal device requested traffic on port `65000`, and instantly drops the packet.
### 3. The Major Disadvantage: Architectural Layer Violation

Purists argue that NAPT violates the fundamental OSI reference model design.

- A router is a **Layer 3 (Network) device**—its only job should be looking at IP addresses.
    
- NAPT forces the router to look inside and alter **Layer 4 (Transport) Port Numbers**.
    

This adds computational overhead to the router, as it has to recalculate packet checksums for every single packet passing through it.