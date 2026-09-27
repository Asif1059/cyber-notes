## Day 3

## OSI & TCP/IP Models — Putting the Network Journey Into Layers

### 1. Topic

**How networking is organized into layers using the TCP/IP and OSI models, and how the concepts I learned previously fit into those layers.**

The main idea of what I learned today is that networking isn't a collection of unrelated things. **DNS, ARP, MAC, switches, IP, routers, and physical signals are different parts of one communication process, organized into layers.** The lesson introduces TCP/IP as the widely adopted model which is actually implemented, and OSI as the model commonly used to describe and troubleshoot networking functions.

---

## 2. What I Learned — One Connected Network Story 🌐

Imagine I open my browser and type:

**`youtube.com`**

The journey can now be understood as both a **network process** and an **OSI-layer process**.

### Step 1 — DNS finds the IP

I start with a **domain name**, not an IP address.

```
youtube.com
     ↓
    DNS
     ↓
IP address
```

**OSI Layer → Layer 7 — Application**

DNS is an application-layer network service, or we can say DNS is an application layer protocol.

---

### Step 2 — Decide: local or remote?

My computer compares the destination IP with its own network.

```
Destination
   ↓
Same network?
   ├── Yes → communicate locally
   └── No  → use default gateway
```

**OSI Layer → Layer 3 — Network**

---

### Step 3 — ARP finds the local MAC address

If the destination is remote, my computer needs the **MAC address of the default gateway** on my local network.

It sends an ARP request like:

> **"Who has 10.1.1.1?"**
> 

The router replies with its MAC address.

ARP connects an IP address to a MAC address on the local network. For exam purposes, ARP is generally treated as operating on the **Layer 2 side** of networking — it's carried directly inside a Layer 2 frame, not inside an IP packet, even though it resolves Layer 3 (IP) info into Layer 2 (MAC) info.

---

### Step 4 — The switch forwards using MAC

Now my computer has the gateway's MAC address.

It creates a Layer 2 frame and sends it through the switch.

The switch looks at the **destination MAC address** and decides where to forward the frame.

```
MAC
 ↓
Switch
 ↓
Local network
```

**OSI Layer → Layer 2 — Data Link**

---

### Step 5 — Router receives the frame

The router receives the frame and looks at the **destination IP address**.

```
    Router
      ↓
 Routing table
      ↓
Destination IP
```

The routing table tells the router **where to send the packet based on its destination IP address**.

**OSI Layer → Layer 3 — Network**

---

### Step 6 — The packet continues through networks

The packet can travel through multiple routers and networks until it reaches the destination network.

On each local network:

```
IP → Layer 3
MAC → Layer 2
Signals → Layer 1
```

So the same overall communication involves **multiple layers working together**.

---

### Step 7 — Physical transmission

Eventually the Layer 2 frame is transmitted physically using:

- Ethernet/cabling
- Network hardware
- Electrical signals

**OSI Layer → Layer 1 — Physical**

---

## 3. New Terms + Meanings

**Networking Model** — A structured way of describing how networking functions work together.

**TCP/IP Model** — The widely adopted networking model used to organize communication into layers.

**OSI Model** — A seven-layer reference model used extensively to describe and troubleshoot networking.

**Layer 1 — Physical** — Physical transmission. *Think: signals, cables.*

**Layer 2 — Data Link** — Local network communication using MAC addresses. *Think: MAC, switch, frame.*

**Layer 3 — Network** — Communication using IP addresses and routing. *Think: IP, router, packet.*

**Layer 4 — Transport** — Transport protocols such as TCP and UDP.

**Layer 5 — Session** — Manages communication sessions.

**Layer 6 — Presentation** — Deals with data representation/formatting (e.g. encoding, compression, encryption in general OSI theory — not something this specific walkthrough traced directly).

**Layer 7 — Application** — Where network applications and application-level protocols operate.

The source emphasizes that OSI layers are still used as everyday terminology even though TCP/IP is the widely adopted model.

---

## 4. The Layer Map You Need to Memorize 🧠

```
Layer 7 → Application
Layer 6 → Presentation
Layer 5 → Session
Layer 4 → Transport
Layer 3 → Network
Layer 2 → Data Link
Layer 1 → Physical
```

### Your networking memory map:

```
L7 → DNS / Applications
L6 → Data format / Encryption
L5 → Sessions
L4 → TCP / UDP
L3 → IP / Router / Routing
L2 → MAC / ARP / Switch
L1 → Signals / Cables
```

For ARP specifically, keep the practical CCNA association:

> **ARP → local IP-to-MAC resolution → Layer 2 side of networking**
> 

---

## 5. What Confused Me

Before, I was seeing things like:

```
DNS
ARP
MAC
Switch
IP
Router
Routing table
```

as separate concepts.

Now I can place them into one process:

```
DNS
 ↓
IP
 ↓
ARP
 ↓
MAC
 ↓
Switch
 ↓
Router
 ↓
Routing table
```

And I can also label them:

```
DNS              → L7
ARP              → L2 side
MAC / Switch     → L2
IP / Router      → L3
Physical signals → L1
```

The important realization is:

> **The OSI model doesn't describe a different network. It gives me a way to organize and describe what is already happening in the network.**
> 

---

## 6. Explain to a Friend 🎯

> When I open a website, DNS helps my computer find the website's IP address. My computer then determines whether the destination is local or remote. If it is remote, ARP helps my computer find the MAC address of its default gateway. The switch uses the MAC address to forward the frame locally, which is Layer 2. The router then looks at the destination IP and uses its routing table to decide where to send the packet, which is Layer 3. Underneath all of this, the data is physically transmitted as signals, which is Layer 1. The OSI model gives us the layer numbers we use to describe all these parts.
> 

---

## 🔥 Day 3 Final Memory Chain

```
DOMAIN
  ↓
DNS
  ↓
IP ADDRESS                    → L7 → DNS/Application
  ↓
LOCAL OR REMOTE?
  ↓
ARP                           → L2 side
  ↓
MAC ADDRESS
  ↓
SWITCH                        → L2
  ↓
DEFAULT GATEWAY
  ↓
ROUTER                        → L3
  ↓
ROUTING TABLE
  ↓
DESTINATION NETWORK
  ↓
PHYSICAL TRANSMISSION         → L1
```

### The 3 most important mappings:

> **DNS → Layer 7**
> 

> **MAC / ARP / Switch → Layer 2**
> 

> **IP / Router / Routing → Layer 3**
> 

And the OSI ladder:

**7 Application → 6 Presentation → 5 Session → 4 Transport → 3 Network → 2 Data Link → 1 Physical**
