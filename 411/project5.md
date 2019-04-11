# Project 5

1. An application is primitive $\iff$ the rator of the application is either a primitive function or an operator.
2. A Jam expression `E` is simple $\iff$ all applications except those nested inside `map` constructions are primitive.



the binary transformer `Cps : Jam * Jam -> Jam` 

the unary transformer `Rsh : Simp -> Simp`



The binary transformer `Cps[k,M]` takes a Jam expression `k` denoting a unary function, and an unshadowed Jam expression `M` as input and produces a tail-recursive Jam expression with the same meaning as `k(M)`.

NOTE: This unshadowing transformation should be applied just after parsing regardless of whether or not the CPS transform is later applied. (This is a requirement for matching parser output in our grading test suite.)





​	