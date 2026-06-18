`OSI`model dictating how all networked devices will send, receive and interpret data
- It consists of 7 layer
- Counted from 7 to 1
- Every layer data travels through, different processes take place

<img width="689" height="471" alt="image" src="https://github.com/user-attachments/assets/4e16df54-2412-40b5-a127-7fc61e9a6c52" />

---

## Application

- Protocols and rules determine how the user should interact with data sent or received (GUI, DNS,...)

---

## Presentation

- Translates data to and from Layer 7

---

## Session

- Create and maintain connection to the other computer for which the data is destined
  - When connection is established, a session is created.
- `Session`is also closing the connection if it hasn't been used in a while or if it is lost

---

## Transport

- Transmitting data across a network
- Follows one of tho different protocols

## TCP (Transmission Control Protocol)

- used for file sharing, internet browsing, sending mails

|Advantages | Disadvantages |
|-----------|---------------|
|Guarantees accuracy of data| Requires a reliable connection. If one chunk of data is not received -> entire chunk of data cannot be used|
|Capable of syncing two devices to prevent data flooding | Slow connection can bottleneck another device |
|Performs a lot of processes for reliability | Very slow|

## UDP (User Datagram Protocol)

- usefull where small pieces of data being sent, e.g. streaming

|Advantages | Disadvantages |
|-----------|---------------|
|Very Fast | Doesn't care if data is received|
|Leaves application layer to decide if there is any control over how quickly packets are sent |Unstable connections result in terrible experience for user |
|Does not need continuous connection | |

---

## Network

- Routing and re-assembly of data takes place
  - Routing means: determine the most optimal path in which the data should be sent
- Routers are known as Layer 3 devices

---

## Data link

- Focuses on physical addressing
- Receives a `Packet` from Network layer and transforms it to a `Frame` adds MAC address of the receiving endpoint

---


## Physical Layer

- References to physical components (e.g. hardware used in networking)
- Devices use electrical signals to transfer data between each other in binary system

---














