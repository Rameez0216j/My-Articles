# Why Does HTTP/3 Use UDP Instead of TCP?  

HTTP/3 uses **UDP instead of TCP** because it is built on **QUIC (Quick UDP Internet Connections)**, which provides **reliability, multiplexing, and improved performance** over traditional TCP.  

## 🚀 Key Reasons  

### 1️⃣ Head-of-Line Blocking in TCP  
- HTTP/2 over TCP suffers from **head-of-line blocking** at the **transport layer**.  
- If one packet is lost, TCP **halts all streams** until the lost packet is retransmitted.  
- QUIC (over UDP) **independently handles lost packets** for different streams, preventing one stream's delay from affecting others.  

### 2️⃣ Built-in Reliability & Encryption  
- Although UDP is unreliable, **QUIC provides its own reliability mechanisms** like retransmissions and acknowledgment similar to TCP.  
- QUIC also includes **TLS 1.3 encryption** by default, reducing the need for extra handshakes.  

### 3️⃣ Faster Connection Establishment  
- TCP requires a **three-way handshake**, adding latency.  
- QUIC reduces this to **one handshake** or even **zero round-trip time (0-RTT)** for repeat connections, improving speed.  

### 4️⃣ Better Performance in Mobile & Changing Networks  
- QUIC uses **connection IDs**, allowing seamless migration between networks (e.g., switching from Wi-Fi to mobile data) **without breaking connections** (unlike TCP).  

### 5️⃣ Reduced Latency for Web Traffic  
- QUIC minimizes **packet loss impact**, leading to **lower latency and improved performance** for web applications, video streaming, and gaming.  

### ✅ Conclusion  
HTTP/3 over **QUIC+UDP** provides the **reliability of TCP** while overcoming its **performance limitations**, making it ideal for modern internet applications.  

---  

# Is HTTP/3 Reliable?  

Yes, **HTTP/3 over QUIC (which uses UDP) is reliable** because QUIC implements **reliability mechanisms similar to TCP**, but at the **application layer** rather than the transport layer.  

## 🔍 Why is HTTP/3 Reliable?  

### 🔄 1. Packet Loss Recovery & Retransmission  
- QUIC **detects lost packets** and **retransmits** them, just like TCP.  
- However, **retransmitted data is not tied to a specific packet number**, avoiding **head-of-line blocking** seen in TCP.  

### 📝 2. Acknowledgment & Flow Control  
- QUIC uses **Selective Acknowledgment (SACK)** to confirm received packets, ensuring reliable data delivery.  
- It also has **flow control** to prevent network congestion.  

### 🔐 3. Encryption & Integrity Checks  
- QUIC includes **TLS 1.3 encryption by default**, ensuring data is **not tampered with** during transmission.  
- It verifies packet integrity, preventing corruption.  

### 🔀 4. Multiplexing Without Blocking  
- QUIC allows multiple streams within a single connection, and **packet loss in one stream does not affect others** (unlike TCP in HTTP/2).  

### 📶 5. Connection Migration Support  
- Since QUIC **uses connection IDs instead of IP+Port**, connections can **seamlessly continue** even if the network changes (e.g., switching from Wi-Fi to mobile data).  

### ✅ Conclusion  
Even though **UDP itself is unreliable**, **QUIC ensures reliability** by handling packet retransmission, acknowledgment, congestion control, and encryption **at the application layer**.  
So, **HTTP/3 over QUIC is just as reliable as TCP but with better performance and lower latency**.  
