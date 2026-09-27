# Week 1 — Networking Fundamentals

This week covered the basics of how networks actually move data, from typing a domain name to a webpage loading in the browser.

## Topics covered

- **Day 1** — DNS resolution, public vs private IP, NAT, ping/traceroute basics
- **Day 2** — Switches, routers, MAC addresses, ARP, how local and remote delivery differ
- **Day 3** — OSI model vs TCP/IP model, mapping real concepts to layers
- **Day 4** — Encapsulation and de-encapsulation (segment → packet → frame), traced through a real example
- **Day 5** — Transport layer, TCP vs UDP, ports, the three-way handshake
- **Day 6** — Hands-on Wireshark: capturing and analyzing real DNS, ARP, ICMP, TCP, and HTTP traffic
- **Day 7** — Revision and first blog post

## Key takeaway

Understanding a concept in theory and recognizing it in real traffic are two different skills. Watching my own packets in Wireshark made the OSI model, encapsulation, and ARP genuinely click in a way reading definitions never did.

## Blog post

Full write-up of the week, including a real debugging investigation into why an HTTP-only site (`neverssl.com`) showed HTTPS traffic in my capture: https://asif-cyber.hashnode.dev/week-1-i-watched-my-own-internet-traffic-here-s-what-i-learned

## Files in this folder

- `day1-networking-basics.md`
- `day2-switches-routers.md`
- `day3-osi-tcpip.md`
- `day4-encapsulation.md`
- `day5-transport-layer.md`
- `day6-wireshark.md`
- `blog-post.md`

## Next up

Week 2: Linux fundamentals and the command line.
