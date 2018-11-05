#COMP 322 HW 3 REPORT

####Design Descriptions

**Ideal Parallel Scoring Design Description**

I initialize the smith-waterman matrix (S) to be a matrix of data driven futures containing integers. I put the starting values into each DDF in the first row and column of S. I then loop over every inner entry in S and for each I create an asyncAwait that waits for the three DDFs that it needs (directly above, directly to the left, and directly to the top-left). Inside the async await the actual value is calculated in the usual way (same as sequential) and put into the current DDF so that anything waiting on it can use it. All the asyncAwaits are wrapped in a finish. After the finish, the bottom-right value of S is returned.

It is data-race free because each DDF in S is written to by a single asyncAwait and the dependency tree we create with the DDFs and async-awaits ensures that it cannot be read from until *after* it is written to.

**Useful Parallel Scoring Design Description**

The smith-waterman matrix (S) is a matrix of integers. A matrix (D) is created containing data-driven futures. The dimensions of D are some relatively small constant (in my case 151x151). The first row and column of S are initialize as in the sequential version and each DDF in the first row and column of D has the value *true* put in it. 

For each inner entry in D an asyncAwait is created, waiting on the three DDFs it needs (directly above, directly to the left, and directly to the top-left). Then, inside each async await, a chunk of S is calculated sequentially (you can think of how the chunks are determined as if you were to map D onto S) and the corresponding DDF is marked *true*. This results in less parallelism than the Ideal implementation above but far less overhead as only a small number of DDFs are created.

It is data race free for similiar reasons to the Ideal implementation. Each value of S can only be written to by a single async await and no other async await can read from it until after it has been written to.

**Sparse Parallel Scoring Design Description**

A matrix D of DDFs is created just as in the Useful Implementation however each DDF contains the last row and the last column of a chunk of the matrix S (there is not explicit matrix S it is divided between many DDFs). Then for each DDF an async await is created just as in the Useful Implementation. The async await computes it's chunk and when it is done, only stores the last row and column in it's corresponding DDF. After each chunk is computed the chunk to its top-left is marked null so that it can be garbage collected as it will never be read from again. This results in much better memory utilization as the only thing that ever needs to be in memory is the chunks currently being operated on three chunks for each of those (top-left, left, top).

It is data-race free for the same reasons as the previous two implementations. Each value can only be written to by a single task and can't be read from until after it is written to.

####Results

| Method                | Input Length | Execution Time |
| --------------------- | ------------ | -------------- |
| SeqScoring.java       | 10,000       | 1751 ms        |
| UsefulParScoring.java | 10,000       | 112 ms         |
| SeqScoring.java       | 100,000      | 412966 ms      |
| SparseParScoring.java | 100,000      | 14692 ms       |
|                       |              |                |

Note: all execution times were copied from autograder run 33293.