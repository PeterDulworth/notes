# PROXY

**Telnet**

```bash
telnet <hostname> <port_number>
```

example:

```bash
> telnet www.example.com 80	
Trying 93.184.216.34...
Connected to www.example.com.
Escape character is '^]'.
GET http://www.example.com HTTP/1.0


```

GET requests are terminated by a new line so you have to press enter twice.

**cUrl**

cUrl actually build the HTTP request for you so once you are done testing invalid requests you can use it.

```bash
curl --proxy http://localhost:port www.example.com
```

e.g.

```bash
curl --proxy sky.clear.rice.edu:18012 www.example.com
```

- redirect output using >, for non-text files
- don't forget that binary data is not the same as text data
  - be careful when using functions that are meant to operate only on strings. they will not work properly with binary data.

the internet has lots of binary data (images, videos,...) which has 0s in the middle of the data. be careful not to use the string functions that see a 0 and assume its the end. i.e. strcpy vs. memcpy (strcpy is null terminated).

**Sockets**

a socket is just a file descriptor 

**Threads**

Threads run within the same process context.

- arbitrary interleaving and parallelization similiar to processes from shell.
- context switches are faster because more is shared between threads, so there is less data to switch (there is less context to be switched).

Threads have their own:

- thread ID
- stack, stack pointer (local variables aren't shared between them when in a function)
- instruction pointer, registers
- status codes

Threads share:

- code sections
- heap
- open files

### HTTP

**HTTP Requests**

An HTTP request consists of a *request line* followed by zero or more *request headers*, followed by an empty text line that terminates the list of headers. A request line has the form:

`method URI version`

e.g. `GET / HTTP/1.1`

Request headers have the form:

`header-name: header-data`

e.g. `Host: www.aol.com`

For out purposes, the only header to be concerned with is the Host header which is required in HTTP/1.1 requests, but not HTTP/1.0 requests.

The empty text line terminates the headers and instructs the server to send the requested HTML file.

> NOTE: HTML requires lines to end with both a carriage return and a line feed (\r\n in C notation).

**HTTP Responses**

An HTTP response consists of a *response line* followed by zero or more *response headers* followed by an empty line that terminates the headers, followed by the *response body*. A response line has the form:

`version status-code status-message`

e.g. `HTTP/1.0 200 OK`

**SERVING DYNAMIC CONTENT**





