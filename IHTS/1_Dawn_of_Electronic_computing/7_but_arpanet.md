# ARPANET and TCP/IP Timeline

ARPANET and TCP/IP are foundational elements of the modern internet, but their development occurred in stages.

## ARPANET (1969)
- The **Advanced Research Projects Agency Network (ARPANET)** was established in **1969** by the **U.S. Department of Defense**.
- It was the first packet-switching network and the precursor to the modern internet.
- The first successful message was sent between UCLA and Stanford Research Institute on **October 29, 1969**.

## TCP/IP Development (1970s)
- **1972**: ARPANET was demonstrated publicly, showcasing email as a major application.
- **1973-1974**: Vinton Cerf and Robert Kahn developed the **Transmission Control Protocol (TCP)**.
- **1978**: TCP was split into **TCP and IP (Internet Protocol)**.
- **January 1, 1983**: ARPANET officially switched to **TCP/IP**, marking the birth of the modern internet.

### Clarification
Although ARPANET started in **1969**, TCP/IP was **not** formed in 1972. Instead:
- **1972** was when ARPANET became widely recognized.
- **TCP was conceptualized in 1973 and finalized by 1978**.
- **The full transition to TCP/IP happened in 1983**.

Thus, ARPANET predated TCP/IP, and the protocol evolved later to replace the earlier **Network Control Protocol (NCP)**.

# TCP vs. IP: Rules Checked When Sending "Hi"

When you send "Hi" to your friend over a network, both **TCP (Transmission Control Protocol)** and **IP (Internet Protocol)** play a crucial role in ensuring the message is delivered correctly.

## **1. Rules Checked by TCP**
TCP is responsible for reliable, ordered, and error-checked delivery. It checks:

### **Connection Establishment (Three-Way Handshake)**
- **SYN:** Your device sends a synchronization (SYN) request to establish a connection.
- **SYN-ACK:** The recipient acknowledges with a SYN-ACK response.
- **ACK:** Your device sends an ACK to confirm the connection.

### **Data Transmission**
- **Segmentation:** TCP divides "Hi" into packets (if necessary).
- **Sequencing:** Assigns sequence numbers to packets to ensure they arrive in order.
- **Acknowledgment:** Confirms receipt of each packet (ACK messages).
- **Retransmission:** Resends lost packets if no acknowledgment is received.
- **Flow Control:** Uses the sliding window mechanism to control data flow.
- **Error Checking:** Uses checksums to detect corruption.

### **Connection Termination**
- **FIN-ACK Exchange:** Ensures proper connection teardown.

---

## **2. Rules Checked by IP**
IP is responsible for addressing and routing packets across networks. It checks:

### **Packet Handling**
- **Source & Destination Addressing:** Ensures packets have correct sender and receiver IP addresses.
- **Fragmentation & Reassembly:** Splits large packets for transmission and reassembles them at the destination.
- **Routing:** Determines the best path to deliver the packet.
- **TTL (Time to Live):** Prevents packets from looping indefinitely.
- **Checksum:** Verifies header integrity but does not guarantee delivery.

---

## **Summary**
| Feature        | TCP Rules                              | IP Rules                         |
|---------------|--------------------------------|--------------------------------|
| **Purpose**   | Reliable communication        | Routing and delivery          |
| **Connection**| Establishes connection (handshake) | No connection (stateless)     |
| **Data Flow** | Ordered, ensures all packets arrive | Unordered, best-effort delivery |
| **Error Handling** | Retransmission, acknowledgment | Header checksum only |
| **Addressing** | Port numbers (application-level) | IP addresses (network-level) |

TCP ensures that "Hi" reaches your friend correctly, while IP ensures it gets routed to the correct destination.

