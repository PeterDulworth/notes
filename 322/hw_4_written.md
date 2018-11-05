# COMP 322 HW 4 WRITTEN

**HJ isolated constructs vs. Java atomic variables**

1. It does have the same semantics because both methods use successive calls to the nextInt method. There are no data-races because lines 4-9 use atomic integers.
2. Because compareAndSet doesn't always guarantee that the value of the seed changes.

**Dining Philosophers Problem**

1. No there is no deadlock possible.
2. No there is no live lock possible. There is starvation but not all the philosophers starve without any of them being blocked.