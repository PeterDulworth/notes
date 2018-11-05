# Networking

internet is a network of networks

Internet is the internet

local area networks (LAN) are connected to each other by routers thus creating a network of networks (internet)

there is a standard protocol for transferring information

Global IP Internet

- based on the TCP/IP protocol family
  - IP (internet protocol):
    - provides basic naming scheme and unreliable delivery capability of packets from host-to-host
  - UDP (Unreliable datagram protocol)
    - uses IP to provide *unreliable* datagram delivery from process-to-process
  - TCP (transmission control protocol) - 99% of internet traffic
    - uses IP to provide *reliable* byte streams from process-to-process over connections

#### IPV4

1. hosts are mapped to a set of 32-bit *IP addresses*
   - each 

### DNS

heirarchal and completely decentralized



```bash
nslookup www.cs.rice.edu
```



##API

Server side:

1. socket: application buffer allocation
2. bind: kernal binds socket to service (via port #)
3. listen: this will be the server socket
4. accept: receive connection request

Client side:

1. connect: set a connection to a server