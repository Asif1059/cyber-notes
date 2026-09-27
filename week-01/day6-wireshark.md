## Day 6

## 1. Topic

I learned how to use **Wireshark to see the networking concepts I had already learned happening in real traffic**.

My main journey was:

**DNS → IP → ARP → ICMP → TCP → HTTP → Ethernet**

---

## 2. What I learned

I started by running:

```bash
ping google.com
```

<img width="1917" height="552" alt="image" src="https://github.com/user-attachments/assets/47fd9463-b891-4aea-89a6-049811e12ab3" />


I then saw DNS traffic resolving:

```
google.com
    ↓
142.251.221.206
```

I also saw two DNS requests for the same domain:

```
A     → IPv4 address
AAAA  → IPv6 address
```

I also saw an ARP request:

<img width="1826" height="126" alt="image" src="https://github.com/user-attachments/assets/3aad1a98-ae9b-4b41-b461-0cec1ffd92b5" />


```
Who has 10.0.2.2?
```

and the reply:

```
10.0.2.2 is at 52:54:00:12:35:00
```

Then I saw the ICMP traffic from the ping:

<img width="1837" height="396" alt="image" src="https://github.com/user-attachments/assets/e1383ab0-341f-4208-8fdd-f8fbbd24fe4d" />


```
10.0.2.15
    ↓
Echo Request
    ↓
142.251.221.206
    ↓
Echo Reply
    ↓
10.0.2.15
```

After that, I opened:

```
http://neverssl.com
```

<img width="1837" height="396" alt="image" src="https://github.com/user-attachments/assets/4033bcb0-397c-406b-8c71-134cf154ee73" />


I filtered for:

```
tcp.flags.syn==1
```

and found the TCP SYN packet from my machine and the SYN-ACK reply from the NeverSSL:

<img width="1916" height="778" alt="image" src="https://github.com/user-attachments/assets/49f5e702-3ed3-4696-b3de-4913656b9b8b" />


```
SYN
↓
SYN-ACK
```

Then I filtered for:

```
http
```

and found:

<img width="1917" height="253" alt="image" src="https://github.com/user-attachments/assets/92402949-d213-4107-bdb9-8f1e4bd8b437" />



```
GET /
```

followed by:

```
HTTP/1.1 200 OK
```

Finally, I opened the HTTP packet details and saw:

<img width="1507" height="166" alt="image" src="https://github.com/user-attachments/assets/c4569e17-1ed9-4f5e-a4a4-0d6da8a2f874" />


```
Ethernet
   ↓
IPv4
   ↓
TCP
   ↓
HTTP
```

---

## 3. New terms + meanings

| Term | Meaning |
| --- | --- |
| Wireshark | Tool I used to capture and inspect network packets |
| A record | DNS request for an IPv4 address |
| AAAA record | DNS request for an IPv6 address |
| ICMP Echo Request | Packet sent by my ping |
| ICMP Echo Reply | Response to my ping |
| ARP | Used to find the MAC address associated with a local IP |
| TCP SYN | Packet seen at the beginning of a TCP connection |
| SYN-ACK | Response to the TCP SYN |
| HTTP GET | Request made to retrieve a web resource |
| 200 OK | Successful HTTP response |
| Source port | Temporary port used by my computer — applies to TCP/UDP traffic (e.g. the HTTP traffic I captured), not ICMP |
| Destination port | Port used by the remote service — applies to TCP/UDP traffic, not ICMP |
| PTR | DNS record used for a reverse IP-to-name lookup |

**Note:** ICMP (ping) traffic doesn't use port numbers at all — ports are a Transport Layer (TCP/UDP) concept, and ICMP operates directly over IP without a transport-layer header.

---

## 4. Commands / filters I used

### Terminal

```bash
ping google.com
```

<img width="1917" height="552" alt="image" src="https://github.com/user-attachments/assets/5b5a5c18-d73d-4c7d-96bb-c799b7bba399" />


I used this to generate the traffic I investigated in Wireshark. (tested against google.com)

```bash
nslookup google.com
```

<img width="1917" height="552" alt="image" src="https://github.com/user-attachments/assets/28945f1a-369c-4e44-b33e-8a746a483bb3" />


I used this to perform a DNS lookup. (tested against google.com)

### Wireshark

```
dns
```

<img width="1917" height="177" alt="image" src="https://github.com/user-attachments/assets/07cdf112-9b6f-4a64-bf84-9a7833fefa0d" />


I used this to find DNS queries and responses. (tested against google.com)

```
icmp
```

<img width="1837" height="396" alt="image" src="https://github.com/user-attachments/assets/3b14fd7d-4033-4e29-bcf4-4e4a3f8506af" />


I used this to find the ping traffic. (tested against google.com)

```
arp
```

<img width="1821" height="101" alt="image" src="https://github.com/user-attachments/assets/5f1b28cd-e703-4870-bcc7-f9b87141cbda" />

I used this to find the ARP request and reply. (tested against google.com)

```
tcp.flags.syn==1
```

<img width="1896" height="661" alt="image" src="https://github.com/user-attachments/assets/7ffb3f9a-fe8b-42e4-b881-ff2f5ca617d0" />


I used this to find TCP SYN and SYN-ACK packets from NeverSS.

```
http
```

<img width="1896" height="661" alt="image" src="https://github.com/user-attachments/assets/a0770f4e-89d4-4975-91ba-137de5332f58" />


I used this to find the HTTP traffic from NeverSSL.

---

## 5. What confused me

At first I was confused about why I saw both `A` and `AAAA` requests for the same `google.com`.

I learned:

```
A     → IPv4 address
AAAA  → IPv6 address
```

I also noticed that the DNS traffic itself could appear over both IPv4 and IPv6.

I was also confused by the TCP traffic appearing in the same capture as my `ping` traffic. Looking at the packets helped me separate the different traffic I was seeing.

I also saw a PTR query such as:

```
206.221.251.142.in-addr.arpa
```

and I learned that this is used for a reverse lookup — going from an IP address back to a domain name.

---

## 6. Explain to a friend

> I opened Wireshark and generated some network traffic. I first looked at DNS and saw `google.com` being resolved to `142.251.221.206`. I also saw A and AAAA requests. Then I saw ARP finding the MAC address for `10.0.2.2`.
> 
> 
> After opening NeverSSL, I found TCP SYN and SYN-ACK packets. Then I filtered for HTTP and found a `GET /` request followed by `200 OK`.
> 
> When I opened the HTTP packet details, I could see Ethernet, IPv4, TCP, and HTTP together. This helped me connect the concepts I learned earlier to actual packets in Wireshark.
> 

---

# Hands-on — Day 6

I performed the experiment in this order:

```
ping google.com
        ↓
DNS traffic
        ↓
google.com → 142.251.221.206
        ↓
ARP traffic
        ↓
ICMP traffic
        ↓
http://neverssl.com
        ↓
TCP SYN / SYN-ACK
        ↓
HTTP GET /
        ↓
HTTP 200 OK
```

My most useful packet was the HTTP packet, where I could see:

```
Ethernet
   ↓
IPv4
   ↓
TCP
   ↓
HTTP
```

### My final memory chain

```
Domain
  ↓
DNS
  ↓
IP address
  ↓
ARP
  ↓
MAC
  ↓
Ethernet
  ↓
TCP / ICMP
  ↓
Port
  ↓
HTTP
  ↓
Data
```

**Day 6 takeaway:** I used Wireshark to connect the networking concepts I learned earlier with the actual packets appearing on my network.
