# Network Fundamentals
a comprehensive document for understanding the fundamental concepts of networking

## OSI Layer 4: Transport Layer
Layer 4 in OSI model provides:
  - **Transparent transfer** of data between hosts.
  - **End-to-end recovery** and **Flow control**: Responsible for end-to-end error recovery and flow control (_**flow control**_ is the process of adjusting the flow of data from the sender to ensure that the receiver can handle all of it).

**Flow control** - For example: If the sender is sending too quickly, maybe because we've got faster network connections on that side, so it's sending more than receiver can accept. If _**flow control**_ is enabled, the receiver will have a mechanism to signal back to the sender, telling it to slow down.
  - **Session Multiplexing**: a process by which a host is able to support multiple sessions simultaneously and manage the individual traffice stream over a single link.

For example:
![image](https://github.com/loihoang1411/Network_Fundamentals/assets/126635820/090f48e1-fc93-46c0-a9d1-c5cb8f51fb16)
The sender sends some emails (SMTP traffic) to the top receiver on port 25, and it also sends some web traffic to the bottom receiver on port 80, and it also sends email traffic to the bottom receiver on port 25 as well. -> We've got 3 sessions from the sender.

--> Layer 4 is responsible for tracking and keeping control of the different sessions on a host.

  - **Port numbers:** are used to identify the upper layer protocol (For example: HTTP uses port 80, SMTP email uses port 25).

The sender also adds a source port number to the Layer 4 header. The combination of source and destination port number can be used to track sessions.  
![image](https://github.com/loihoang1411/Network_Fundamentals/assets/126635820/9a314d28-e0ff-420c-b2a0-0dfdf9ffa291)
_**How a stateful firewall works?**_ We've got a connection between the sender and receiver. When the receiver sends traffic back, it will flip the source and the destination port numbers around. If a firewall is in the middle rather than a switch, assume that we had a rule in the firewall that said traffic is allowed out from the sender (on the left) out to the network (on the right), but traffic is not allowed from the right to the left unless it was initiated from the sender, in this case, we're allowing from the sender to the receiver, that traffic is allowed outbound. When the return traffic comes back, the firewall could see based on the source and destination port numbers (-> it's return traffic going back sender again) -> the firewall alows this traffic to come through. If the traffic had been initiated by the reciever, the firewall would not allow that traffic.
  - **TCP (Transport Control Protocol)**: **1)** TCP is connection oriented (once a connection is established, data can be sent bidirectionally over that connection). **2)** TCP carries out sequencing to ensure segments are processed in the correct order and none are missing. **3)** TCP is reliable - the receiver sends acknowledgments back to the sender. Lost segments are resent. **4)** TCP performs flow control.

The TCP Three-Way Handshake:
![image](https://github.com/loihoang1411/Network_Fundamentals/assets/126635820/9d623fdc-7992-4d58-913f-140cf8bb2408)
The sender is going to initiate the connection. It sends a SYN, a Synchronized Message, over to the receiver. When the receiver receives that, it will send a SYN-ACK back (a Synchronized Acknowledgment). Finally, to complete the connection, the sender will send an Acknowledgment (ACK).

OSI packet encapsulation:
![image](https://github.com/loihoang1411/Network_Fundamentals/assets/126635820/7d6bd7c5-e3a2-4f02-b209-a6e9077c01c7)
First, the packet starts at Layer 7. It will then encapsulate that with the Layer 6 header and then gets encapsulated with the Layer 5 header, then the Layer 4 header, then the Layer 3 header, then the Layer 2 header and then we send it on to the physical wire.

TCP header:
![image](https://github.com/loihoang1411/Network_Fundamentals/assets/126635820/edeceb8d-86f6-47cf-8585-95d65431cad1)
**1)** Source Port, Desination Port. **2) and 3)** Sequence Number and Acknowledgment Number. **4)** Header Length. Reserved field: for any reserved information later. Code Bits, Window, which can be used for flow control. **5)** Checksum, which can be used to check and see if the traffic got corrupted in transit. Urgent. **6)** Options. **7)** Data.

  - **UDP (User Datagram Protocol)**: **1)** UDP is not connection oriented. There is no handshake connection setup between the hosts. **2)** UDP does not carry out sequencing to ensure segments are processed in the correct order and none are missing. **3)** UDP is not reliable - the receiver host does not send acknowledgments back to the sender. **4)** UDP does not perform flow control. **5)** If error detection and recovery is required, it is going to be up to the upper layers to provide it (or higher, up to the application level to actually provide that, it's not going to be provided by UDP).

UDP header:
![image](https://github.com/loihoang1411/Network_Fundamentals/assets/126635820/864bd57f-0ad0-4b0e-845a-8fcbea9a1b91)
**1)** Source Port and Destination Port. **2)** Length and UDP checksum. **3)** Data.

---> Application developers will typically choose to use TCP for traffic which requires reliability. For example: FTP - File Transfer Protocol (21), SSH - Secure Shell (22), Telnet (23), HTTP - web traffic (80), HTTPS - encrypted web traffic (443).

---> Real-time applications such as voice and video, which can't afford the extra overhead of TCP or the extra overhead is slow the real-time traffic down, and it's going to affect the quality. So they use UDP. For example: TFTP - Trivial File Transport Protocol (69), SNMP - Simple Network Management Protocol (161).

---> Some applications can use both TCP and UDP. For example: DNS (53)

