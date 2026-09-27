## Day 1 :

## 1. Topic: How the Internet Works DNS, IP, Ping, Traceroute

## 2. What I learned

- IP addresses uniquely identify a device on a network so that the device can send or receive data.
- **DNS** translates human readable domain names (like `google.com, youtube.com etc.`) into the IP address that can actually locates the server.
- DNS finds the IP address by follows this chain when the answer is not cached: **stub resolver → recursive resolver → root server → TLD server → authoritative DNS server**, with each step narrowing down who to ask next.
- Devices use the **subnet mask** to check whether a destination IP is on the same local network or a different network.
- If the destination is outside the local network, the device sends the traffic to its **default gateway also known as** router, which forwards it further and does the same steps until destination is reached .
- **Private IPs** only work inside a local network. Devices sharing same internet connection show up as a single **public IP** to the outside world, thanks to a router process called **Network Address Translation or simply NAT**.
- `ping` tests whether a destination is reachable and measures round trip time, using ICMP.
- `tracert`/`traceroute` reveals the number of hops /routers data or message passes through on the way to a destination. some routers may not respond, which doesn't mean the path is broken.

## 3. New terms + meaning

- **Stub Resolver** : it is the DNS client on your device. it checks its local cache first, then asks a DNS resolver if it doesn't know the answer.
- **Recursive Resolver** : it is a DNS server (e.g., your router ) that does the work of asking other servers on your behalf until it finds the IP address.
- **ICMP** : it is Internet Control Message Protocol, used for diagnostics and error reporting. `ping` and `traceroute` both rely on it.
- **NAT (Network Address Translation) : it is** the process a router uses to let many devices with private IPs share one public IP when talking to the internet.

## 4. Commands used + what they showed

| Command | What it showed |
| --- | --- |
| `ping <address>` | Tests reachability and shows round-trip response time via ICMP |
| `nslookup <domain>` | Queries DNS and returns the IP address or addresses associated with a domain |
| `ipconfig /all` | Shows local network config: private IPv4/IPv6, subnet mask, default gateway, DNS servers, MAC address |
| `tracert <address>` | Shows the hop by hop path to the destination, including any non responding hops |

## 5. What confused me

- I initially thought DNS resolution establishes the connection to the website, i was wrong. DNS only finds the IP address, the actual data exchange with the server happens afterward through some other process .
- I also assumed `ping` was part of the process of loading a website, i was wrong. `ping` is just a diagnostic tool, and a browser doesn't need to ping a server before connecting to it.
- also I initially thought `ping` is used to establishing a connection between the host and server , but it doesn't. it just checks reachability and latency using one off ICMP request/reply messages.

## 6. This is how i will explain it to a friend

When I type a website name, DNS looks up its IP address for me, once the IP address is found. Then my computer checks "is this IP address on my own network, or somewhere else?" It does that by comparing the destination IP address against a rule called the subnet mask, kind of like checking if a phone number has the same area code as mine. If it matches, the destination is right here on my local network. If it doesn't match, my computer hands the traffic off to my router, which forwards it toward the destination and hides my private IP behind its own public one on the way out. `ping` just checks if something responds and how fast, while `tracert` shows me the actual path the data takes to get there.
