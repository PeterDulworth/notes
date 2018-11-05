# Threads and Locks (Unit 7)

## 7.1 Threads

Normal:

```python
finish {
	async S1
    S2
}
S3
```

Thread Version:

```python
T1 = new Thread(S1) # start a thread executing S1
T1.Start
S2
T1.Join
S3
```

> overthread in setting up threads is 1000x more at least than setting up tasks

so you set up a pool of threads once at the start of the program then you create tasks on those threads in the program at much lower overhead.

threads are a much more low level form of programming and thus much more error prone.

## 7.2 Structured Locks

```python
SYNCHRONIZED(L1) {
    # aquire L1
    
    # release L1
}
```

like isolated, synchronized guarantees that two threads cannot simultaneously aquire the same lock, they must wait in turns.

unlike isolated, synchronized is lower level and the programmer actually dictates which resources are actually used.