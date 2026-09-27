## Day 4

### 1. Topic

**Following a packet through the OSI layers as it travels from a computer to a web server.**

Used Packet Tracer to watch a web request move through **Layers 7 → 4 → 3 → 2 → 1**, then get **de-encapsulated** at the destination.

---

## 2. What I Learned — One Connected Network Journey 🌐

Johnny wants to visit a website.

```
Website request
      ↓
Application
      ↓
Transport
      ↓
Network
      ↓
Data Link
      ↓
Physical
      ↓
Switch
      ↓
Router
      ↓
Another network
      ↓
Server
```

### Step 1 — Application Layer

The browser wants a website.

For a secure website, **HTTPS is used**.

**Layer 7 → Application → HTTP/HTTPS**

### Step 2 — Transport Layer

The data is given transport information.

The example uses:

**TCP + destination port 443 (HTTPS)**

A **Layer 4 header** is added.

Now the data is called a:

> **Segment**
> 

### Step 3 — Network Layer

The Layer 3 header adds:

- Source IP
- Destination IP

Now the segment becomes a:

> **Packet**
> 

**Layer 3 → IP → Router**

### Step 4 — Data Link Layer

Now MAC addresses are added.

The frame contains the local delivery information:

**Source MAC → Destination MAC**

Now the packet becomes a:

> **Frame**
> 

**Layer 2 → MAC → Switch**

### Step 5 — Physical Layer

The frame is transmitted physically through the network.

Think:

> **Ethernet cable → electrical signals**
> 

**Layer 1 → Physical**

---

## Then the network devices do their jobs

### Switch

The switch works with the **Layer 2 information**.

It looks at the destination MAC address and uses its CAM/MAC address table to determine where to forward the frame.

> **Switch → Layer 2 → MAC**
> 

### Router

The router removes the Layer 2 information and looks at the **Layer 3 destination IP**.

Then it checks its routing table.

> **Router → Layer 3 → IP → Routing table**
> 

The router then creates a **new Layer 2 frame** for the next network.

The source and destination MAC addresses change for that new local network, while the Layer 3 IP information is used for the packet's destination.

---

## At the destination

The server receives the frame and works back upward:

```
Frame
  ↓
Packet
  ↓
Transport data
  ↓
Application data
```

This is:

> **De-encapsulation**
> 

The server checks:

**Layer 2 → Is this my MAC?**

Then:

**Layer 3 → Is this my IP?**

Then:

**Layer 4 → Which port/service?**

Then:

**Layer 7 → What application data is this?**

---

## 3. New Terms + Meanings

**Encapsulation** — Adding information as data moves **down** the layers.

**De-encapsulation** — Removing/interpreting that information as data moves **up** the layers.

**Segment** — Layer 4 data.

**Packet** — Layer 3 data.

**Frame** — Layer 2 data.

**Layer 4 Header** — Contains transport information such as **TCP/UDP and port numbers**.

**Layer 3 Header** — Contains **source and destination IP addresses**.

**Layer 2 Header** — Contains **source and destination MAC addresses**.

**Trailer** — Added at Layer 2 alongside the header; typically contains the **FCS (Frame Check Sequence)**, used to detect transmission errors.

### Big chain to memorize

```
Application Data
      ↓
   Segment       → Layer 4
      ↓
   Packet        → Layer 3
      ↓
    Frame        → Layer 2
      ↓
Physical Signals → Layer 1
```

---

## 4. Commands / Tools Used

### Packet Tracer — Simulation Mode

The main hands-on tool I used to track the data through the layers.

It lets you step through the packet and inspect what happens at each layer.

### Important things to observe

**Application → HTTPS**

**Transport → TCP / Port 443**

**Network → Source + Destination IP**

**Data Link → Source + Destination MAC**

**Physical → Ethernet transmission**

---

## 5. What Confused Me

The important thing that becomes clearer here is:

> **The entire message is not simply sent unchanged from computer to server.**
> 

Instead:

```
Application data
      ↓
Transport header + Application data = segment
      ↓
IP header + segment = Packet
      ↓
MAC header + packet + trailer = frame
      ↓
    Frame
      ↓
Physical transmission
```

And when the router receives it:

```
Frame
 ↓
Remove Layer 2 information
 ↓
Inspect Layer 3
 ↓
Routing decision
 ↓
Create a NEW Layer 2 frame
 ↓
Send onward
```

So the **Layer 2 information can change as the packet moves between networks**, while the router uses the Layer 3 destination IP to determine where the packet needs to go.

---

## 6. Explain to a Friend 🧠

> When I visit a website, my computer starts with application data. At the Transport Layer, TCP and a port such as 443 are added, creating a segment. At the Network Layer, source and destination IP addresses are added, creating a packet. At the Data Link Layer, MAC addresses are added, creating a frame. The frame is then transmitted physically. The switch uses the MAC address to forward the frame, while the router uses the destination IP and routing table to decide where the packet should go. When the packet reaches the server, the server de-encapsulates everything until the original application data is reached.
> 

---

## 7. Worked Example — Visiting google.com

| OSI Layer | What happens when I visit `google.com`? |
| --- | --- |
| **Application — L7** | DNS resolves `google.com` to an IP address. My browser then creates an **HTTPS request**. |
| **Transport — L4** | **TCP** is used. The destination port is **443**. TCP information is added, creating a **segment**. |
| **Network — L3** | My computer checks the destination IP against its subnet mask. It's outside my local network, so the **default gateway becomes the next hop**. A Layer 3 header with source and destination IP addresses is added, creating a **packet**. The IP addresses stay the same for the whole journey. |
| **Data Link — L2** | The computer needs the default gateway's MAC address, found via **ARP**. The packet is wrapped in an **Ethernet frame** with source/destination MAC addresses. This frame gets rebuilt with new MAC addresses at every hop along the way. |
| **Physical — L1** | The frame is transmitted as physical signals (electrical over copper, light over fiber, or radio over Wi-Fi). If the device is already transmitting, the new frame waits in a buffer. |

**Key takeaway to remember:** the packet's IP addresses stay constant end-to-end, but the frame's MAC addresses change at every hop — this is why ARP has to run again at each router along the path.

### The key chain demonstrated by this example

```
HTTPS
  ↓
TCP : 443
  ↓
Destination IP
  ↓
Outside local subnet
  ↓
Default Gateway
  ↓
ARP → Gateway MAC
  ↓
Ethernet Frame
  ↓
Physical transmission
```
