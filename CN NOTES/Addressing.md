## ADDRESSING
### 1. IP ADDRESS
Every device needs an address so that other devices can find it. that is called an IP ADDRESS.
two versions:
#### 1. IPv4 (internet protocol version 4)
assigns a unique, 32-bit numerical label to every device connected on the network.
###### FORMAT: 
Four numbers (called octets) separated by periods 192.168.1.1
##### Range:
Each octet range from 0 to 255 2^8 possible values per octet.
##### Total Address space:
2^32 which is around 4.3 billion unique addresses.
#### COMPONENT BREAKDOWN:
IPv4 divided into two distinct parts based on the subnet:
**1**. **Network ID: ** identifies the specific network the device belongs to.
2.Host ID: identifies the specific device within the network.

### IPv4 Address classes
5 classes A-E to allocate address spaces efficiently. This is known as **CLASSFUL ADDRESSING**.


| CLASS       | FIRST OCTET RANGE | SUBNET MASK   | PURPOSE                                     |
| ----------- | ----------------- | ------------- | ------------------------------------------- |
| **CLASS A** | 1-126             | 255.0.0.0     | massive networks(huge corporations)         |
| **CLASS B** | 128-191           | 255.255.0.0   | Medium to large networks                    |
| **CLASS C** | 192-223           | 255.255.255.0 | SMALL local networks                        |
| **CLASS D** | 224-239           | N/A           | Multicasting (streaming ,routing protocols) |
| **CLASS E** | 240-255           | N/A           | Experimental/ reserved for future use       |


#### NOTE:
127.X.X.X is omitted from the table because it is reserved for **loopback testing** (testing network software on your local machine)
2. Subnet mask define a Network id and Host id for routing. Class D and E do not have subnet mask because they are never split into a network and a host.
### SPECIAL AND RESERVED IP ADDRESSES

##### 1. Private IP Addresses
These are used within a private local area networks (LANs) and cannot be directly reached from internet
1. 10.0.0.0 to 10.255.255.255
2. 172.16.0.0 to 172.31.255.255
3. 192.168.0.0 to 192.168.255.255
##### 2. APIPA (AUTOMATIC PRIVATE IP ADDRESSING)
feature when a device cannot obtain an IP address from a DHCP server.
##### HOW IT WORKS
1. a device starts and requests ip address from a dhcp server
2. if the dhcp server is unavailable pr does not respond, the device automatically assigns itself an IP address.
3. The assigned IP address is from the range : **169.254.0.0 - 169.254.255.255** with a subnet mask : **255.255.0.0**
## THE BIG CHALLENGE : ADDRESS EXHAUSTION
4.3 BILLION ARE NOT ENOUGH FOR TODAY'S INTERNET. 
1. **CIDR( CLASSLESS INTER-DOMAIN ROUTING)**: replaced the rigid class system, allowing addresses to be allocated in variable-sized blocks to minimize waste.
2. **NAT (NETWORK ADDRESS TRANSLATION): ** allows an entire home or corporation to share a single public IPv4 address to access the internet.
3. **IPv6: ** It uses 128-bit addresses, providing 3.4 * 10^38 unique addresses.