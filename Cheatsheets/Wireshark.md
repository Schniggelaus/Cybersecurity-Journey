# Wireshark Filter Cheatsheet

Wireshark is used to capture and analyse network traffic.

---

## Filtering Operators

| Operator | Definition |
|---------|-----------|
| `and` or `&&` | and |
| `or` or `\|\|` | or |
| `eq` or `==` | equal |
| `ne` or `!=` | not equal |
| `gt` or `>` | greater than |
| `lt` or `<` | less than |

---

## Basic Filters

| Filter | Description |
|--------|-------------|
| `ip.addr == <IP>` | Show all packets involving the defined IP address |
| `ip.src == <IP>` | Show packets from a specific source IP |
| `ip.dst == <IP>` | Show packets to a specific destination IP |
| `ip.src == <SRC> and ip.dst == <DST>` | Show traffic between two specific IPs |
| `tcp.port eq <Port>` | Filter by TCP port number |
| `udp.port eq <Port>` | Filter by UDP port number |

---

## Protocol Filters

| Filter | Description |
|--------|-------------|
| `http` | Show only HTTP traffic |
| `dns` | Show only DNS traffic |
| `tcp` | Show only TCP traffic |
| `udp` | Show only UDP traffic |
| `arp` | Show only ARP traffic |
| `icmp` | Show only ICMP traffic |

---

## Protocol-Specific Filters

| Filter | Description |
|--------|-------------|
| `tcp.flags.syn == 1` | Show TCP SYN packets (start of 3-way handshake) |
| `icmp.type == 8` | Show ICMP Echo Requests (ping sent) |
| `icmp.type == 0` | Show ICMP Echo Replies (ping response) |
| `arp.opcode == 1` | Show ARP Requests |
| `arp.opcode == 2` | Show ARP Replies |
| `http.request` | Show HTTP requests only |
| `http.response` | Show HTTP responses only |
| `dns.flags.response == 0` | Show DNS queries |
| `dns.flags.response == 1` | Show DNS responses |

---

*Extended as new protocols and filters are encountered.*
