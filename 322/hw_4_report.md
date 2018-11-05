# COMP 322 HW 4 REPORT

**Design Description**

In order to maximize parallelism, I created $n$ threads, where $n$ is the number of threads passed into the $runIteration$ function. Each thread runs the sequential algorithm with slight modification outlined below:

I changed the $nodesLoaded$ list to be a concurrent queue so that multiple threads can operate on it safely. In order to keep the threads from coliding, I added a lock field to the ParComponent class so that each node would have its own lock. This allowed me to share the work of the while loop but at the same time ensure that all parallel work is always happening on disjoint $(n, other)$ pairs.

My implementation is data-race free and can't result in live-lock or dead-lock because before operating on any node, I always check that nodes lock, to make sure that no other thread is operating on that node.



**Correctness Test Output:**

```
======= STDOUT =======
Running edu.rice.comp322.BoruvkaCorrectnessTest
JUnit version 4.12
.Testing parallel minimum spanning tree construction on src/main/resources/boruvka/USA-road-d.NY.gr.gz
.Testing parallel minimum spanning tree construction on src/main/resources/boruvka/USA-road-d.BAY.gr.gz
.Testing parallel minimum spanning tree construction on src/main/resources/boruvka/USA-road-d.COL.gr.gz

Time: 12.178

OK (3 tests)



======= STDERR =======
```



**Performance Test Output**

- 1 Thread:

  ```
  JUnit version 4.12
  .
  HABANERO-AUTOGRADER-PERF-TEST 1T edu.rice.comp322.BoruvkaPerformanceTest.testInputUSAroadNE 1676 1670
  === For dataset src/main/resources/boruvka/USA-road-d.NE.gr.gz ===
  Single-threaded test ran in 1676 ms, 0.7583532219570406x faster than sequential (1271 ms)
  1 threaded test ran in 1670 ms, 1.0035928143712576x faster than single-threaded (1676 ms)


  Time: 124.252

  OK (1 test)
  ```

- 2 Threads:

  ```
  JUnit version 4.12
  .
  HABANERO-AUTOGRADER-PERF-TEST 2T edu.rice.comp322.BoruvkaPerformanceTest.testInputUSAroadNE 1674 1047
  === For dataset src/main/resources/boruvka/USA-road-d.NE.gr.gz ===
  Single-threaded test ran in 1674 ms, 0.7562724014336918x faster than sequential (1266 ms)
  2 threaded test ran in 1047 ms, 1.5988538681948423x faster than single-threaded (1674 ms)


  Time: 97.244

  OK (1 test)
  ```

- 4 Threads:

  ```
  JUnit version 4.12
  .
  HABANERO-AUTOGRADER-PERF-TEST 4T edu.rice.comp322.BoruvkaPerformanceTest.testInputUSAroadNE 1711 753
  === For dataset src/main/resources/boruvka/USA-road-d.NE.gr.gz ===
  Single-threaded test ran in 1711 ms, 0.7387492694330801x faster than sequential (1264 ms)
  4 threaded test ran in 753 ms, 2.2722443559096948x faster than single-threaded (1711 ms)


  Time: 91.43

  OK (1 test)
  ```

- 6 Threads:

  ```
  JUnit version 4.12
  .
  HABANERO-AUTOGRADER-PERF-TEST 6T edu.rice.comp322.BoruvkaPerformanceTest.testInputUSAroadNE 1706 670
  === For dataset src/main/resources/boruvka/USA-road-d.NE.gr.gz ===
  Single-threaded test ran in 1706 ms, 0.7403282532239156x faster than sequential (1263 ms)
  6 threaded test ran in 670 ms, 2.546268656716418x faster than single-threaded (1706 ms)


  Time: 87.884

  OK (1 test)
  ```

- 8 Threads:

  ```
  JUnit version 4.12
  .
  HABANERO-AUTOGRADER-PERF-TEST 8T edu.rice.comp322.BoruvkaPerformanceTest.testInputUSAroadNE 1683 676
  === For dataset src/main/resources/boruvka/USA-road-d.NE.gr.gz ===
  Single-threaded test ran in 1683 ms, 0.7344028520499108x faster than sequential (1236 ms)
  8 threaded test ran in 676 ms, 2.489644970414201x faster than single-threaded (1683 ms)


  Time: 87.348

  OK (1 test)
  ```

  ​

