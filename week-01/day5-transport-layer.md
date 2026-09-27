## Day 5

### 1. Topic

**I learned what happens at the upper OSI layers when I use YouTube, how TCP and UDP transport my data, and how port numbers identify services.**

I learned about:

**Application → Presentation → Session → Transport → TCP/UDP → Ports**

The lesson connects these concepts to actual YouTube traffic and Wireshark.

---

# 2. What I Learned — My Network Journey 🌐

When I open my browser and go to:

```
youtube.com
```

### Application Layer — L7

I use my browser to communicate with YouTube.

```
My Browser
    ↓
Application Layer
    ↓
"I want YouTube."
```

The browser creates an application-level request.

### Presentation Layer — L6

My data needs to be represented in a format that the other side can understand.

I learned that this layer deals with things such as:

**Data formatting + encryption**

### Session Layer — L5

My browser and the YouTube server have a logical communication session.

```
My Browser ↔ YouTube
```

The session is started, maintained, and eventually ended.

### Transport Layer — L4

Now I have to decide **how my data should be transported**.

I learned about:

```
TCP → Reliable
UDP → Faster / less reliability-focused
```

With TCP, I first establish a connection using:

```
SYN
 ↓
SYN-ACK
 ↓
ACK
```

Then I can exchange data. TCP can also retransmit data when needed.

### Ports

I also learned that the Transport Layer uses port numbers.

```
IP address → Which host?
Port        → Which service?
```

For HTTPS, I learned:

```
Port 443 → HTTPS
```

My computer can also use a temporary **ephemeral port** for its side of the communication.

### Then I continue down the stack

```
Application Data
      ↓
Transport information
      ↓
Segment
      ↓
IP information
      ↓
Packet
      ↓
MAC information
      ↓
Frame
      ↓
Physical transmission
```

This connects directly to what I learned in Episode 4 about encapsulation.

---

# 3. New Terms + Meanings

### TCP

I learned that **TCP** focuses on reliable communication, including verification and retransmission.

### UDP

I learned that **UDP** avoids TCP's connection and reliability process and can be useful when speed matters.

### Three-Way Handshake

```
SYN → SYN-ACK → ACK
```

I use this to establish a TCP connection before normal data exchange.

### Port

I learned that a port helps identify the particular service I want on a host.

### Well-Known Ports

I learned that the lesson treats:

```
0–1023
```

as the well-known port range.

### Ephemeral Port

I learned that my computer can use a temporary client-side port for a communication.

### Encapsulation

I add information as my data moves **down** through the layers.

### De-encapsulation

The receiving side processes that information as the data moves **up** through the layers.

---

# 4. Commands / Tools Used + What They Showed

### Wireshark

I learned how Wireshark can capture my real network traffic.

I used it to observe how my computer and YouTube communicate with each other:

```
Source IP
Destination IP
TCP
Three-way handshake
Port 443
UDP traffic

```

| What I identify | My observation |
| --- | --- |
| Website | `youtube.com` |
| Remote server | 142.251.153.4 |
| Port | 443 |
| Protocol | **TCP and UDP(QUIC)** |
| TCP or UDP | Both |

### Important ports I learned

```
21   → FTP        (TCP)
22   → SSH        (TCP)
23   → Telnet     (TCP)
25   → SMTP       (TCP)
53   → DNS        (TCP and UDP)
69   → TFTP       (UDP)
80   → HTTP       (TCP)
123  → NTP        (UDP)
443  → HTTPS      (TCP, and UDP if QUIC/HTTP3)
3389 → RDP        (TCP, primarily)
```

---

# 5. What Confused Me

At first, I might think:

> **"Web traffic always uses TCP."**
> 

But the lesson showed me that communication with YouTube can involve **both TCP and UDP**.

So my mental model is now:

```
Application
    ↓
Transport
    ↓
TCP or UDP
    ↓
Ports
    ↓
Network
    ↓
IP
```

I also learned not to think:

> **Video = always UDP**
> 

Instead, I should look at the actual traffic and see which protocol is being used.

---

# 6. Explain to a Friend 🧠

> "When I open YouTube, my browser creates application data. The presentation layer deals with formatting and encryption, and the session layer represents my communication session. At Layer 4, my data is transported using TCP or UDP and uses port numbers to identify the service. For HTTPS, port 443 is commonly used. TCP establishes a connection using SYN, SYN-ACK, and ACK and focuses on reliable delivery, while UDP avoids TCP's connection and reliability process and can be useful when speed matters. Then my data continues down to the Network, Data Link, and Physical layers."
> 

---

# 🔥 7. My Day 5 Hands-On

## Browser Developer Tools

I will now inspect real traffic from my own browser.

### Step 1

I open Chrome and press:

```
F12
```

Then I select:

**Network**

### Step 2

I load:

```
https://youtube.com
```

### Step 3

I let the page load and select a request related to the website/video.

### Step 4

I try to identify:

```
Remote server
Port
Protocol
TCP or UDP
HTTP/2 or HTTP/3
```

### What I expect to learn

For normal HTTPS communication, I may see:

```
:443
```

I may also discover something interesting:

```
HTTP/2 → TCP
```

or possibly:

```
HTTP/3
   ↓
QUIC
   ↓
UDP
```

So instead of simply memorizing **"TCP is used for websites,"** I can actually look at my own traffic and see what is happening.

---

# 🧠 My Day 5 Final Memory Chain

```
I open YouTube
      ↓
APPLICATION
"What do I want?"
      ↓
PRESENTATION
"How is my data represented/protected?"
      ↓
SESSION
"Which communication session?"
      ↓
TRANSPORT
"How should I transport it?"
      ↓
TCP / UDP
      ↓
PORT
"Which service?"
      ↓
NETWORK
"Which IP?"
      ↓
DATA LINK
"Which MAC locally?"
      ↓
PHYSICAL
"Send it."
```

### My most important mappings

**Application → Browser / application protocols**

**Presentation → Data format / encryption**

**Session → Communication session**

**Transport → TCP / UDP / Ports**

**TCP → SYN → SYN-ACK → ACK → Reliable delivery**

**UDP → Faster communication without TCP's connection/reliability process**

**Port 443 → HTTPS**

**Ephemeral port → Temporary client-side port**

**Encapsulation → Down**

**De-encapsulation → Up**
