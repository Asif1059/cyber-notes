## Day 2 :

### 1. Topic

**How a device communicates locally, crosses into another network, and finally reaches a website/server using switches, MAC addresses, ARP, the default gateway, routers, routing tables, and DNS.**

---

### 2. What I learned

Imagine **Johnny wants to visit a website from his computer**.

First, Johnny types a domain name such as:

```
www.youtube.com
```

but his computer can communicate with a website only when it know the  websites IP address.

his computer doesn't initially know the websites IP address. That's where **DNS** comes in:

```
Domain name -> DNS -> IP address
```

The DNS server gives his computer the IP address of the website.

Now his computer knows the destination IP, so the next question is:

> **Is that destination on his own network or somewhere else?**
> 

If the destination is on Johnny's own network, Johnny’s computer needs the destination device's **MAC address**, because the local switch works at Layer 2 using MAC addresses.

To find that MAC address, his computer uses **ARP(address resolution protocol)**:

```
Known: IP address -> ARP -> Needed: MAC address
```

his computer broadcasts (i.e. sends to all devices in the network) an ARP request asking:

> "Who has this IP address?"
> 

The device that has that IP replies with its MAC address.

Now his computer can create a **Layer 2 frame** containing:

```
Source MAC and Destination MAC , Source IP and Destination IP.
```

The switch receives the frame and uses the **destination MAC address** to decide where to forward it. The switch learns MAC information and stores it in its **CAM table**.

So locally, the flow is:

```
IP -> ARP -> MAC -> Frame -> Switch -> Connected Device
```

But now imagine Johnny's destination is **not on his network**.

For example:

```
Johnny → 10.1.1.xping 

Website → 23.227.38.x
```

his computer realizes:

> "That device isn't in my network."
> 

So his computer doesn't try to find the destination server's MAC address directly.

Instead, his computer needs his **default gateway**, which is his router.

So Johnny uses ARP again, but this time for the router:

```
“Who has 10.1.1.1 (this is the default gateway’s/routers IP address)?” → The router replies with its MAC address.
```

Now his computer creates a frame like:

```
Source MAC      = Johnny
Destination MAC = Router

Source IP      = Johnny
Destination IP = Website
```

This is the key connection:

> **MAC addresses tell the local network where to send the frame. IP addresses tell the network where the packet finally needs to go.**
> 

The switch therefore sends the frame to the router.

The router receives it and examines the **destination IP address**. Unlike the switch, the router is a **Layer 3 device**, so it works with IP addresses.

Now the router asks:

> "Which network contains this destination IP?"
> 

To answer that, it checks its **routing table**.

For example:

```
10.1.1.0/24      → G0/0
23.227.38.0/24   → G0/1
```

So the router knows which interface leads toward the destination network.

The router now needs to deliver the packet onto that next network.

If it doesn't know the destination device's MAC address there, it performs **ARP on that network**:

```
"Who has the destination IP?" -> Destination replies ->  Router learns MAC
```

The router can then create a new Layer 2 frame for that network and send the packet onward.

So the complete journey becomes:

```
              Johnny's Computer
                     │
              Domain name entered
                     │
                     ▼
                    DNS
                     │
               Gets destination IP
                     │
                     ▼
            Is destination local?
               │             │
              Yes            No
               │             │
             ARP        ARP for gateway
               │             │
               ▼             ▼
             MAC          Router
               │             │
               ▼             ▼
            Switch       Routing table
                             │
                             ▼
                       Other network
                             │
                            ARP
                             │
                             ▼
                      Destination server
```

That's how all the concepts from this lesson connect.

---

### 3. New terms + meanings

**MAC address** → the Layer 2 address used for delivery within the local network. its the permanent address of the device within the network.

**ARP** → finds MAC address from IP address, because MAC address is needed by switch to deliver the frame within the network.

**Switch** → uses MAC addresses to move frames within a network.

**CAM table** → the switch's learned map of MAC addresses and ports.

**Default gateway** → the router a device uses when the destination is outside its own network.

**Router** → connects different IP networks.

Routing table → tells the router where to send a packet based on its destination IP address.

**DNS** → connects a human readable domain name to an IP address.

---

### 4. Commands used + what they showed

```bash
ping <IP>
```

Tests connectivity to another IP address.

```bash
enable
```

Enters privileged Cisco CLI mode.

```bash
show mac-address-table
```

Lets you see the switch's learned MAC-address-to-port information.

```bash
show ip route
```

Lets you see the router's map of reachable networks and their paths/interfaces.

---

### 5. What confused me

> I was treating **MAC, IP, ARP, switch, router, DNS, and routing table** like separate topics. Now I can connect them as one process:
> 
> 
> **DNS gives me the IP → computer determine whether the destination is local or remote → ARP gives me the local MAC → the switch delivers the frame → the router is used for remote networks → the routing table tells the router where to send the packet → ARP can happen again on the next network.**
> 

---

### 6. Explain to a friend

> When I open a website, DNS first helps my computer find its IP address. If the website is outside my network, my computer uses ARP to find the MAC address of my default gateway, then sends the frame through the switch to the router. The router checks the destination IP in its routing table and forwards the packet toward the correct network. On each local network, MAC addresses and ARP handle the actual Layer 2 delivery.
> 

### The one chain to remember

```
DOMAIN
  ↓
DNS
  ↓
IP ADDRESS
  ↓
LOCAL OR REMOTE?
  ↓
ARP
  ↓
MAC ADDRESS
  ↓
SWITCH
  ↓
DEFAULT GATEWAY
  ↓
ROUTER
  ↓
ROUTING TABLE
  ↓
DESTINATION NETWORK
  ↓
SERVER

```

---
