# Packets & Frames

## TCP/IP

- `TCP` (Transmission Control Protocol)
- Similar to OSI model, but with less layers:
  - Application
  - Transport
  - Internet
  - Network Interface
- `TCP` is connection-based
- To guarantee that any data sent will be received on the other hand, TCP uses a process called `Three-way handshake`

## Three-way handshake

- uses some special messages:

|Step| Message | DEscription |
|-----|--------|-------------|
|1|`SYN`| `SYN` packet is the first message to initiate the handshake between two devices|
|2|`SYN/ACK`| This packet is sent by the receiving device to acknowledge the synchronisation attempt|
|3|`ACK`| Acknowledgement packet is used by the client/server to acknowledge that a series of packets have been successfully received|
|4|`DATA`| Once connection is established, data is sent via `DATA` message|
|5|`FIN`| Packet is used to cleanly close the connection|
|#|`RST`| Packet abruptly ends all communication|

**Start of the connection**
<img width="789" height="630" alt="image" src="https://github.com/user-attachments/assets/458dfbbb-70e4-4c13-a72f-bba24ec62368" />
**Closing of the connection**
<img width="724" height="617" alt="image" src="https://github.com/user-attachments/assets/d7668fc6-a2eb-4f55-a315-e6f09706c284" />

## UDP (User Datagram Protocol)

- `UDP` is stateless -> therefore no ACK packet is sent during a connection

<img width="429" height="356" alt="image" src="https://github.com/user-attachments/assets/0295b3c7-c48b-436d-98c5-7e137339aaba" />


## Ports

- There are set Ports for different situations

| Protocol | Port Number | Description|
|----------|-------------|------------|
|File Transfer Protocol (FTP)|21| File-sharing application build on client-server model -> it is possible to download files from|
|SSH|22| Secure login to systems |
|Telnet|23| interact with remote computers or network devices with TCP|
|HTTP|80| Protocol used by WWW - Browser uses this to download text, images,...|
|HTTPS|443| Same as HTTP, but more secure using encryption|
|Server Message Block (SMB)|445| similar to FTP, SMB allows to share devices like printers|
|Remote Desktop Protocol (RDP)|3389| Used to logging in to a system using a visual desktop interface|


