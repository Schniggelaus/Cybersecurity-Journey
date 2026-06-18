# What is Networking?

**Learnings:**

- Every Network has a *public* IP-Address
- Every Device in a *public* IP-Address has it´s own *private* IP-Address
- IPv4 got extended to IPv6 due to a lack of availability of different public IP-Addresses
- MAC address is a unique address which is regarding to a micriochip located in the device´s motherboard
  - They can be `spoofed`
- `ICMP` (Internet Control Message Protocol) determine performance of a connection

## LAN Topology

`Start Topology`
  - Devices are individually connected
  - Most common today cause of it's reliability and scalability
  - <img width="936" height="514" alt="image" src="https://github.com/user-attachments/assets/58a3ac08-801a-4b45-9baf-8b91622b8d8d" />


`Bus Topology`
  - Connection relies on single backbone cable
  - <img width="927" height="447" alt="image" src="https://github.com/user-attachments/assets/0f41ff52-a43b-40be-8add-f9bd38325387" />

`Ring Topology`
  - Sends data across the loop until it reaches the destined device
  - <img width="672" height="486" alt="image" src="https://github.com/user-attachments/assets/0530b680-a85d-4bfc-ba50-ed646ff72acf" />

|LAN Topologie |Advantages| Disadvantages|
|----------|--------------|-------------|
|Star Topology|Scaleable|More Expensive; The bigger the network, the bigger the maintenance/ harder troubleshooting|
|Bus Topology| Cost efficient; little redundancy in failures| High chances of bottlenecked; difficult troubleshooting (hard to identify what device has an issue|
|Ring Topology| Easy troubleshooting, Less prone to bottlenecks| Small fault will break the entire network|

`Switches`
  - Groups mutiple devices and assigning them a port
  - Keep track of what device is connected to what port
  - Can be connected to Router
  - <img width="1078" height="530" alt="image" src="https://github.com/user-attachments/assets/e0f5a83b-cc71-4c0b-875d-83b8f266962a" />

## ARP (Address Resolution Protocol)
- Allows devices allows their MAC address being associated with the IP address on the network
- Functionallity
  - `ARP Request` -> sent -> message is broadcasted on the network asking other devices: "What is the MAC address that owns this IP address?" -> If msg reaches correct device ->
    sent `ARP Reply`

## DHCP (Dynamic Host Configuration Protocol)

- Assigns IP-addresses automatically
  - Device connects to network -> sends request to DHCP Discover to see if any DHCP Servers are on the Network -> Server replies with IP address (DHCP Offer)
    -> device sends a reply confirming (DHCP Request) -> DHCP reply with acknowleding (DHCP ACK)
    
