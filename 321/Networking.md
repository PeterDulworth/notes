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

## API

Server side:

1. socket: application buffer allocation
2. bind: kernal binds socket to service (via port #)
3. listen: this will be the server socket
4. accept: receive connection request

Client side:

1. connect: set a connection to a server



#### `getaddrinfo`

```c
int getaddrinfo(const char* host, 				/* Hostname or address. */
                const char* service, 			/* Port or service name. */
                const struct addrinfo *hints, 	/* Input parameters. */
                struct addrinfo **result)		/* Output linked list. */
    
void freeaddrinfo(struct addrinfo **result)		/* Free linked list. */
    
const char *gai_strerror(int errcode) 			/* Return error message. */
```

#### `addrinfo` struct

```c
struct addrinfo {
    int 				ai_flags; 		/* Hints arg flags. */
    int 				ai_family;		/* First arg to socket function. */
    int					ai_socktype;	/* Second arg to socket function. */
    int 				ai_protocol;	/* Third arg to socket function. */
    char 			   *ai_canonname;	/* Canonical host name. */
    size_t				ai_addrlen;		/* Size of ai_addr struct. */
    struct sockaddr    *ai_addr;		/* Ptr to socket address structure. */
    struct socknext    *ai_next;		/* Ptr to next item in linked list. */
}
```

Establish a connection with a server:

```c
int open_clientfd(char *hostname, char *port) {
    int clientfd;
    struct addrinfo hints, *listp, *p;
    
    memset(&hints, 0, sizeof(struct addrinfo));
    hints.ai_socktype = SOCK_STREAM; /* Open a connection. */
    hints.ai_flags = AI_NUMERICSERV; /* ...using numeric port arg. */
    hints.ai_flags |= AI_ADDRCONFIG; /* Recommended for connections. */
    getaddrinfo(hostname, port, &hints, &listp);
}
```



### 3. Thread based servers

process = process context + code, data and stack