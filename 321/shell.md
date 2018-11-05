# Shell

**Tips**:

1. Try running the command "printenv".  It shows the entirety of the current environment.  In other words, all of the currently defined environment variables.  I expect that you'll see a lot more than just PATH. 

   Any shell, yours included, should pass the entire environment to the newly started program.  Ultimately, when your shell is written and debugged, running the command "printenv" under your shell should produce similar output to running that command under the default shell on CLEAR.

   Within any program, including your shell, the global variable "environ" provides access to the entire environment.

2. signal() just modifies the default actions associated with that signal

   > handler_t *signal(int signum, handler_t *handler)

   Different values for handler 

   * SIG_IGN
   * SIG_DFL
   * otherwise handler points to a function defined by the user

3. kernal blocks any pending signals of type currently being handled

4. children inherit the blocked bit-vector of their parents

**Guidelines for safe signal handlers**

- keep your signal handlers as simple as possible

- call only async-signal-safe functions from your handlers

  > printf, malloc, exit are not signal safe!

- save and restore errno on entry and exit

  > so that other handlers don't overwrite 

- protect access to shared data structures by temporarily blocking all signals.

  > if you are accessing any shared data structures so that no other handlers mess you up

- declare any global variables that are shared between the signal handlers and the main routine as volatile

  > to prevent compiler from storing in register

- declare global flags as volatile sig_atomic_t


**Async-Signal-Safety**



### Chapter 8

**Concurrent Flows:**

Flows X and Y are concurrent with respect to each other if and only if X begins after Y begins and before Y finishes, or Y begins after X begins and before X finishes.

```c
pid_t getpid(void) // returns pid of current process
pid_t getppid(void) // returns pid of parent process
```

Three States of Processes: 

1. Running (Runable)
2. Stopped
3. Terminated

```c
void exit(int status) // terminates process with exit status of status
pid_t fork(void) // returns 0 to child, PID of child to parent, and -1 on error
```

fork creates a child process almost identical to the parent (but seperate).

#### Reaping

When the parent reaps the terminated child, the kernel passes the child's exit status to the parent and then discards the terminated process, at which point it ceases to exist.

 A terminated process that has not yet been reaped is called a zombie.

```c
pid_t waitpid(pid_t pid, int *statusp, int options) //returns PID of child if OK, 0 (if WNOHANG), or -1 on error
```

**waitpid options:**

1. WNOHANG. Return immediately (with a return value of 0) if none' of thechild processes in the wait set has terminated yet. The default behaviorsuspends the calling process until a child terminates; this option is usefulin those cases where you want to continue doing useful work while waitingfor a child to terminate.
2. WUNTRACED. Suspendexecutionofthecallingprocessuntilaprocessinthewait set becomes either terminated or stopped. Return the PID of theterminated or stopped child that caused the return. The default behaviorreturns only for terminated children; this option is useful when you wantto check for both terminated and stopped children.
3. WCONTINUED. Suspend execution of the calling process until a runningprocess in the wait set is terminated or until a stopped process in the waitset has been resumed by the receipt of a SIGCONT signal. (Signals areexplained in Section 8.5.)

**checking the exit status of a reaped child:**

1. WIFEXITED(status). Returns true if the child terminated normally via a call to exit or a return.
2. WEXITSTATUS(status). Returns the exit status of a normally terminated child. This status is only defined if WIFEXITED() returned true.
3. WIFSIGNALED(status). Returns true if the child process terminated because of a signal that was not caught.
4. WTERMSIG(status). Returns the number of the signal that caused the child process to terminate. This status is only defined if WIFSIGNALED() returned true.


5. WIFSTOPPED(status). Returns true if the child that caused the return is currently stopped.
6. WSTOPSIG(status). Returns the number of the signal that caused the child to stop. This status is only defined if WIFSTOPPED returned true.
7. WIFCONTINUED(status). Returns true if the child process was restarted by receipt of a SIGCONT signal.
















> The functions *waitpid*, *kill*, *fork*, *execve*, *setpgid*, and *sigprocmask* will be the fun-damental building blocks of your shell. Make sure that you understand what these functions do and how to use them. The *WUNTRACED* and *WNOHANG* options to *waitpid* will also be useful.

**waitpid**

```c
pid_t waitpid(pid_t pid, int *statusPtr, int options);
```

If `pid` is greater than 0, the calling process waits for the process whose process identification number (PID) is equal to the value specified by `pid`. 

If `pid` is equal to 0, the calling process waits for the process whose process group ID is equal to the PID of the calling process.

If `pid` is equal to -1, the calling process waits for any child process to terminate.

If `pid` is less than -1, the calling process waits for the process whose process group ID is equal to the absolute value of `pid`.

**kill**

**fork**

**execve**

```c
int execve(const char *filename, const char *argv[], const char *envp[]); // -1 on error else none
```



**setpgid**

```c
int setpgid(pid_t pid, pid_t pgid); // 0 on success, -1 on error
```



**sigprocmask**





TODO:

- [ ] can't enter commands of longer than 9 characters to be parsed by search path
- [ ] do i need to malloc "found"?
- [ ] comments
- [ ] consolidate print statements
- [ ] what signal blocking needs to happen in do_bgfg??

