# C330 HW3

## Peter Dulworth

### Code

```python
import numpy as np
import scipy.stats

# one coin has a probability of coming up heads of 0.2, the other 0.6
coinProbs = np.zeros (2)
coinProbs[0] = 0.2
coinProbs[1] = 0.6

# reach in and pull out a coin numTimes times
numTimes = 100

# flip it numFlips times when you do
numFlips = 10

# flips will have the number of heads we observed in 10 flips for each coin
flips = np.zeros (numTimes)
for coin in range(numTimes):
        which = np.random.binomial (1, 0.5, 1); # randomly choose which coin you take out of the sack
        flips[coin] = np.random.binomial (numFlips, coinProbs[which], 1);

# initialize the EM algorithm
coinProbs[0] = 0.79
coinProbs[1] = 0.51

def calculate_c(i, j, p1, p2):
	pz = p1 if j == 0 else p2
	num = 0.5 * scipy.stats.binom.pmf(flips[i], numFlips, pz) 
	denom = 0.5 * ( scipy.stats.binom.pmf(flips[i], numFlips, p1) + scipy.stats.binom.pmf(flips[i], numFlips, p2) )
	return num / denom

def calculate_p(whichP, p1, p2):
	num_sum = 0.0
	denom_sum = 0.0
	for i in range(numTimes):
		num_sum += calculate_c(i, whichP, p1, p2) * flips[i]
		denom_sum += calculate_c(i, whichP, p1, p2) * numFlips
	return num_sum / denom_sum

# run the EM algorithm
for iters in range (20):
	newP0 = calculate_p(0, coinProbs[0], coinProbs[1])
	newP1 = calculate_p(1, coinProbs[0], coinProbs[1])
	coinProbs[0] = newP0
	coinProbs[1] = newP1
	print coinProbs[0], "\t", coinProbs[1]
```



### Results

**numFlips = 10:**

```
0.709107005856 	0.345131001438
0.668253134224 	0.279810810694
0.646901127483 	0.248671901817
0.636051444735 	0.23361341575
0.630607680688 	0.226424446629
0.627915062751 	0.223010008282
0.626600875144 	0.22138732927
0.625965409934 	0.220614693143
0.625659840555 	0.220246229474
0.625513351527 	0.220070340828
0.62544323637 	0.219986333587
0.625409703452 	0.219946198969
0.625393672516 	0.219927021822
0.625386010159 	0.219917857934
0.625382348101 	0.219913478776
0.625380597979 	0.219911386067
0.625379761603 	0.219910385996
0.625379361905 	0.219909908078
0.625379170894 	0.219909679687
0.625379079613 	0.219909570542
```

**numFlips = 2**

```
0.580258021964 	0.271604381625
0.539330956842 	0.236759921083
0.533376424277 	0.229562792618
0.533463666378 	0.227154835612
0.534425754607 	0.225765865096
0.535383152979 	0.224719521264
0.53620453177 	0.223871510544
0.536888003088 	0.223174070163
0.537451970405 	0.222599680158
0.537915774205 	0.222127231992
0.538296455878 	0.221739248621
0.538608464942 	0.221421083384
0.538863899889 	0.221160485335
0.539072827058 	0.220947250101
0.539243586528 	0.220772911689
0.539383065807 	0.220630469904
0.539496938173 	0.220514152528
0.539589867022 	0.220419210628
0.539665679181 	0.220341744322
0.539727510687 	0.220278555763
```



