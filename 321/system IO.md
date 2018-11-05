# SYSTEM I/O (ch.10)

A linux file is a sequence of $m$ bytes:

$B_0, B_1,...,B_k,...,B_{m-1}$

All I/O devices are represented as files:

- /dev/sda2 (/usr disk partition)
- /dev/tty2 (terminal)

Even the kernal is represented as a file:

- /boot/vmlinuz-3.13.0-55-generic (kernal image)
- /proc (kernal data structures)

#### Simple interface called *unix I/O*:

- opening and closing files: `open()` and `close()`
- reading and writing files: `read()` and `write()`
- Changing the current file positiong (seek
  - indicates next offset into file to read or write
  - `lseek()`

Each file has a *type* indicating its role in the system:

- Regular file: contains arbitrary data
- Directory: index for a related group of files
- socket: for communicating with a process on another machine
- ... named pipes (FIFOS), symbolic links, character and block devices...

#### Regular Files

- a regular file contains arbitrary data
- applications often distinguish between *text files* and *binary files*
  - text files are regular files with only ASCII or Unicode characters
  - binary files are everything else
  - kernal doesn't know the difference!
- Text file is a sequence of text *lines*
  - text line is a sequence of chars terminated by newline char ('\n')
    - newline is 0xa, same as ASCII line feed char (LF)
- End of line (EOL) indicators in other systems
  - Linux and Mac OS: '\n' (0xa)
    - line feed (LF)
  - windows and internet protocols: '\r\n' (0xd 0xa)
    - carriage return (CR) followed by line feed (LF)

#### Directories

- directory consists of an array of *links*
  - each link maps a filename to a file
- each directory contains at least two entries
  - `.` (dot) is a link to itself
  - `..` (dot dot) is a link to the *parent directory* in the *direcotyr heirarchy*
- kernal maintains *current working directory* (cwd) for each process

#### Opening Files

- opening a file informs the kernal that you are getting ready to access that file

  ```c
  int fd; /* file descriptor */

  if ((fd == open("/etc/hosts", O_RDONLY)) < 0) {
      perror("open");
      exit(1);
  }
  ```

- returns a small identifying integer **file descriptor**

  - `fd == -1` indicates that an error occurred 

- each process created by a linux shell begins life with three open files associated with a terminal:

  - `0`: standard input (stdin)
  - `1`: standard output (stdout)
  - `2`: standard error (stderr)

#### Closing Files

- closeing a file informs the kernal that you are finished accessing that file 

```c
int fd; /* file descriptor */
int retval; /* return value */

if ((retval = close(fd)) < 0) {
    perror("close");
    exit(1);
}
```

- closing an already closed file is a recipe for disaster in threaded programs
- moral: aways check return codes, even for seemingly benign functions such as close()

#### Reading Files

- reading a file copies bytes from the current file position to memory, and then updates file position

  ```c
  char buf[512];
  int fd; /* file descriptor */
  int nbytes; /* number of bytes read */
      
  /* open file fd... */
  /* then read up to 512 bytes from file fd */
  if ((nbytes = read(fd, buf, sizeof(buf))) < 0) {
      perror("read");
      exit(1);
  }
  ```

- returns number of bytes read from file *fd* into *buf*

  - returns type `ssize_t` is signed integer
  - `nbytes < 0` indicates that an error occured
  - short counts (`nbytes < sizeof(buf)`) are possible and are not errors!

#### Writing Files

- writing a file copies bytes from memory to the current file position, and then updates current file position

  ```c
  char buf[512];
  int fd;
  int nbytes;

  /* Open the file fd... */
  /* Then write up to 512 bytes from buf to file fd */
  if ((nbytes = write(fd, buf, sizeof(buf))) < 0) {
      perror("write");
      exit(1);
  }
  ```

- returns number of bytes written from *buf* to file *fd*

  - `nbytes < 0` indicates that an error occured
  - as with reads, short counts are possible and are not errors!

- copying `stdin` to `stdout`, one bytes at a time

  ```c
  #include "csapp.h"

  int
  main(void)
  {
      char c;
      
      while(Read(STDIN_FILENO, &c, 1) != 0)
          Write(STDOUT_FILENO, &c, 1);
      exit(0);
  }
  ```

#### Short Counts

- short counts can occur in these situations:
  - encountering (EOF) on reads
  - reading text lines from a terminal
  - reading and writing network sockets
- short counts never occur in these situations:
  - reading from disk files (except for EOF)
  - writing to disk files
- best practice is to always allow for short counts

## RIO

##### RIO - Robust Input and Output (wrapper for I/O)

> RIO is a set of wrappers that provide efficient and robust I/O in apps, such as network programs that are subject to short counts.

RIO provides two different kinds of functions:

- unbuffered input and output of binary data
  - `rio_readn` and `rio_writen`
  - `rio_readn` will not return until that number of bytes has been read
- buffered input of text lines and binary data
  - `rio_readlinb` and `rio_readnb`
  - buffered RIO routines are thread-safe and can be interleaved arbitrarily on the same descriptor. 

#### Unbuffered RIO Input and Output

- same interface as unix `read` and `write`

- especially useful for transferring data on network sockets

  ```c
  #include "csapp.h"

  ssize_t rio_readn(int fd, void *usrbuf, size_t n);
  ssize_t rio_writen(int fd, void *usrbuf, size_t n);

  /* Return: num. bytes transferred if OK, 0 on EOF (readn only), -1 on error. */
  ```

- `rio_readn` returns short count only if it encounters EOF 

  - only use it when you know how many bytes to read

- `rio_writen` never returns a short count

- calls to `rio_readn` and `rio_writen` can be interleaved arbitrarily on the same descriptor.

  Implementation of `rio_readn`:

  ```c
  /*
   * rio_readn - robustly read n bytes (unbuffered)
   */
  ssize_t rio_readn(int fd, void *usrbuf, size_t n) 
  {
      size_t nleft = n;
      ssize_t = nread;
      char *bufp = usrbuf;
      
      while (nleft > 0) {
          if ((nread = read(fd, bufp, nleft)) > 0) {
              if (errno == EINTR) /* Interrupted by sig handler return */
                  nread = 0;		/* and call read() again */
              else 
                  return -1;		/* errno set by read() */
          }
          else if (nread == 0)
              break;				/* EOF */
          nleft -= nread;
          bufp += nread;
      }
      return (n - nleft);			/* Return >= 0 */
  }
  ```

#### Buffered IO

```c
#include "csapp.h

void rio_readinitb(rio_t *rp, int fd);

ssize_t rio_readlineb(rio_t *rp, void *usrbuf, size_t maxlen);
ssize_t rio_readnb(rio_t *rp, void *usrbuf, size_t n);

/* Return num bytes if OK, 0 on EOF, -1 on error. */
```

- `rio_lineb` reads a text line of up to `maxlen` bytes from file `fd` and stores the line in `usrbuf`
  - !!! especially useful for reading text lines from network sockets
- stopping conditions
  - `maxlen` bytes read
  - EOF encountered
  - newline encountered

> Idea: instead of every time going to the operating system and asking for some small number of characters, just ask for a lot and store them in local buffer. keep a pointer in the buffer that points to the stuff in the buffer that hasn't yet been read by the application program.

![Screen Shot 2018-04-04 at 1.49.11 AM](/Users/Peter/Desktop/Screen Shot 2018-04-04 at 1.49.11 AM.png)

```c
typedef struct {
    int rio_fd;					/* descriptor for this internal buf */
    int rio_cnt;				/* unread bytes in internal buf */
    char *rio_bufptr;			/* next unread byte in internal buf */
   	char rio_buf[RIO_BUFSIZE];	/* internal buffer */
} rio_t;
```

- Example: copying the lines of a text file from stdin to stdout

  ```c
  #include "csapp.h"

  int
  main(int argc, char **argv) 
  {
      int n;
      rio_t rio;
      char buf[MAXLINE];
      
      Rio_readinitb(&rio, STDIN_FILENO);
      while ((n = Rio_readlineb(&rio, buf, MAXLINE)) != 0)
          Rio_writen(STDOUT_FILENO, buf, n);
      exit(0);
  }
  ```



## How the Linux Kernal represents open files

> these can be tricky exam questions and will likely be on the exam.

![Screen Shot 2018-04-04 at 2.04.37 AM](/Users/Peter/Desktop/Screen Shot 2018-04-04 at 2.04.37 AM.png)

- each process has a desciptor table (one table per process)
- each file has a `refcnt` that lets the operating system know when it is no longer needed
- every file in the system (open or closed) has a vnode entry

If you open a file more than once, you get multiple file descriptors. i.e. you can read from multiple positions in a file at once.

File descriptors might point to the same file.

A child process inherits its parent's open files AFTER fork.

- child's descriptor table same as parent's, and +1 to each `refcnt`. 
- the descriptor tables still point to the same files. i.e. the files do not get duplicated.
- The result of this is that if you move the position pointer of a file in one process, it also gets moved in the other.

**dup2**

`dup2(oldfd, newfd)` copies (per-process) descriptor table entry `oldfd` to entry `newfd`

e.g.: `linux> ls > foo.txt` would be accomplished with `dup2(4, 1)`

#### Aside: working with binary files

- functions you should never use on binary files
  - text-oriented I/O such as `fgets, scanf, rio_readlineb`
    - interpret EOL chars
    - use functions like `rio_readnb, rio_readn` instead
  - string functions
    - `strlen, strcpy, strcat`

