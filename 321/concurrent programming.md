# Concurrent Programming

> Logical control flows are *concurrent* if they overlap in time.

A thread is a logical flow that runs in the context of a process. Each thread has its own thread context, including a unique integer thread ID (TID), stack, stack pointer, program counter, general purpose registers, and condition codes. ALL threads running in a process share the entire virtual address space (including its code, data, heap, shared libraries, and open files) of that process.

Threads are organizaed as a main thread and a pool of peer threads.

**Thread Creation**

```c
#include <pthread.h>
typedef void *(func)(void *);

int pthread_create(pthread_t *tid, pthread_attr_t *attr, func *f, void *arg);
/* Returns: 0 if OK, nonzero on error. */
```

Creates a new thread and runs the thread routine `f` in the context of the new thread and with an input argument of `arg`. The `attr` argument will always be `NULL` for us.

```c
pthread_t pthread_self(void);	/* Returns: thread ID of caller. */
```

**Thread Termination**

4 Ways:

- Implicitly when its top-level thread routine returns.

- Explicitly by calling the `pthread_exit` function. If the main thread calls `pthread_exit`, it waits for all other peer threads to terminate and then terminates the main thread and the entire process with a return value of `thread_return`.

  ```c
  void pthread_exit(void *thread_return); /* Never returns. */
  ```

- Some peer thread calls the Linux exit function, which terminates the process and all threads associated with the process.

- Another peer thread terminates the current thread by calling the `pthread_cancel` function with the ID of the current thread.

  ```c
  int pthread_cancel(pthread_t tid); /* Returns: 0 if OK, nonzero on error. */
  ```

**Reaping Terminated Threads**

Threads wait for other threads to terminate by calling `pthread_join` function.

```c
int pthread_join(pthread_t tid, void **thread_return); /* 0 if ok, else error */
```

The `pthread_join` function blocks until thread `tid` terminates, assigns the generic `(void *)` pointer returned by the thread routine to the location pointed to by `thread_return`, and then reaps any memory resources held by the terminated thread.

Notice that unlike the Linux wait function, the `pthread_join` function can only wait for a specific thread to terminate (there is no way to wait for an arbitrary thread).

**Detaching Threads**

At any point in time a thread is *joinable* or *detached*. 

- Joinable Threads: can be reaped and killed by other threads. Its memory resources (such as stack) are not freed until it is reaped by another thread.
- Detached Threads: cannot be reaped or killed by other threads, Its memory resources are freed automatically by the system when it terminates.

Threads default to joinable.

```c
int pthread_detach(pthread_t tid); /* Returns 0 if OK, nonzero on error. */
```

The `pthread_detach` function detaches the joinable thead `tid`. Threads can detach themselves by calling `pthread_detach` with an argument of `pthread_self()`.

**Initializing Threads**

```c
pthread_once_t once_control = PTHREAD_ONCE_INIT;

int pthread_once(pthread_once_t *once_control, void (*init_routine)(void));
/* Always returns 0. */
```



## Thread Memory Model

Each thread has its own seperate *thread context*, which includes

- Thread ID
- Stack
- Stack Pointer
- Program Counter
- Condition Codes
- General-Purpose Register Values

Each thread shares the rest of the process context with the other threads. This includes the entire user virtual address space:

- read-only text (code)
- read/write data
- the heap
- any shared library code / data areas
- open files

> Different stacks are not protected from other threads. So if a thread somehow manages to aquire a pointer to another thread's stack, then it can read and write any part of that stack.

**Mapping Variables to Memory**

**<u>Global Variables</u>** - A global variable is any variable declared outside of a function. At run time, the read/write are of virtual memory contains exactly one instance of each global variable that can be referenced by any thread.

**<u>Local Automatic Variables</u>** - A local automatic variable is one that is declared inside a function without the `static` attribute. At run time, each thread's stack contains its own instances of any local automatic variables. This is true even if multiple threads execute the same thread routine.

**<u>Local Static Variables</u>** - A local static variable is one that is declared inside a function with the static attribute. As with global variables, the read/write area of virtual memory contains exactly one instance of each local static variable declared in a program. 

**Shared Variables**

We say that a variable $v$ is shared $\iff$ one of its instances is referenced by more than one thread.

> It is important to realize that local automatic variables can also be shared.

## Synchronizing Threads with Semaphores

A semaphore $s$, is a global variable with a nonnegative integer value that can only be manipulated by two special operations, called $P$ and $V$.

- $P(s)$ - If $s$ is nonzero, then $P$ decrements $s$ and returns immediately. If $s$ is zero, then suspend the thread until $s$ becomes nonzero and the thread is restarted by a $V$ operation. After restarting, the $P$ operation decrements $s$ and returns control to the caller.
- $V(s)$ - The $V$ operation increments $s$ by 1. If there are any threads blocked at a $P$ operation waiting for $s$ to become nonzero, then the $V$ operation restarts exactly one of these threads, which then completes its $P$ operation by decrementing $s$.

 



