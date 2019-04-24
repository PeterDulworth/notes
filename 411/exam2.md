# 411 Final Exam Notes

## Types

<https://www.youtube.com/watch?v=QY1tNx97Eek&list=PLbdXd8eufjyXVOzffVYuONivdq7N1uV_U&index=4>

a5: <https://www.cs.rice.edu/~javaplt/411/19-spring/Assignments/5/>

a5 code: <https://github.com/PeterDulworth/rice-comp411/blob/master/hw5/test/Assign5Test.java>

```scheme
let 
    append: (list int, list int -> list int) 
        := map x: list int, y: list int to         
            if x = null: int then y 
            else cons(first(x), append(rest(x), y));     
    s: list int 
        := cons(1,cons(2,cons(3,null: int))); 
in append(s,s)
```



## Dynamic Dispatch

<https://www.cs.rice.edu/~javaplt/411/19-spring/Lectures/19.pdf>



## Continuations

Continuations are just "todo lists". 

"Once you get a value here is what you need to do with it". So continuations take a value as their argument and tell you what to do with them.



CPS: <https://www.cs.rice.edu/~javaplt/411/19-spring/Lectures/25.pdf>







## Garbage Collection

### 1. Reference Counting

Each cell contains an extra "reference count" field.

- Dereferencing -> count ++
- Deleting reference -> count --



Advantages: memory is reclaimed immediatley 

Disadvanages:

- Overhead of constantly changing the reference count
- Can't handle cycles

### 2. Tracing Garbage Collection

Trace from one memory cell to the next to figure out which is live and which is dead.

Start from the top stack frame.

#### 2a. Mark and Sweep Collection

Two phases:

**Phase I** "mark"

traverse the pointer graph and mark all reachable nodes as live (cells must contain a boolean "isAlive")

**Phase II** "sweep"

iterate over the entire heap and if a cell is marked live, un mark it (so that we can do phase I again later), otherwise mark it as empty (we can't delete it because the heap is a list and that would mess up all the memory locations).

<hr/>

each empty cell contains a reference to the next empty cell 

#### 2b. Copying Collector



#### 2c. Generational Collector

