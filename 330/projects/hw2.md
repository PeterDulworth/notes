# C330 HW2

## Peter Dulworth

### Gradient Descent

```python
import numpy as np
import math

def f(x, y):
	"""
	McCormick Function.
	MINIMUM: f(-0.54719755, -1.54719755) = -1.9132229549810367
	"""
	return math.sin(x + y) + (x - y) ** 2 - 1.5 * x + 2.5 * y + 1

"""
The partial derivative of the McCormick function with respect to x.
"""
def x_partial(a):
	return math.cos(a[0] + a[1]) + (2.0 * a[0]) - (2.0 * a[1]) - 1.5

"""
The partial derivative of the McCormick function with respect to y.
"""
def y_partial(a):
	return math.cos(a[0] + a[1]) - (2.0 * a[0]) + (2.0 * a[1]) + 2.5

"""
A gradient descent algorithm to find the minimum of the McCormick function. 
Uses the bold driver heuristic to determine the learning rate.
Input:
	(1) a : NumPy array with two entries (the starting x and y)
"""
def gd_optimize(a):

	prev_theta = a
	prev_L = f(prev_theta[0], prev_theta[1])
	learning_rate = 1.0

	while True:
		cur_theta = prev_theta - (learning_rate * np.array([x_partial(prev_theta), y_partial(prev_theta)]))

		cur_L = f(cur_theta[0], cur_theta[1])

		# at the end of each iteration, print out the value of the objective function
		print cur_L

		# if the change in the objective function is less than 10e-20, 
		# print out the current value for x and y and then exit.
		if abs(cur_L - prev_L) < 10 ** -20:
			print "Minimum:\n(x, y) =", cur_theta, "\n"
			return

		# bold driver heuristic 
 		# if we are getting farther from the minimum, go slower
		# if we are getting closer to the minimum, go faster
		learning_rate = learning_rate * (0.5 if cur_L > prev_L else 1.1)

		prev_theta = cur_theta	
		prev_L = cur_L
```

### Results

```
>> gd_optimize(np.array( [-0.2, -1.0] )) 
-1.31753873182
-1.50326587316
-1.39339295625
-1.90763217732
-1.91290001532
-1.91318075043
-1.9132152451
-1.91322073145
-1.91322174639
-1.91322187729
-1.91322154594
-1.91322294772
-1.91322295475
-1.91322295496
-1.91322295498
-1.91322295498
-1.91322295498
-1.91322295498
-1.91322295498
-1.91322295498
-1.91322295498
-1.91322295498
-1.91322295498
-1.91322295498
-1.91322295498
-1.91322295498
Minimum:
(x, y) = [-0.54719755 -1.54719755]

>> gd_optimize(np.array( [-0.5, -1.5] )) 
-1.91092958058
-1.91146816749
-1.9110297007
-1.91322152817
-1.91322292147
-1.91322295258
-1.91322295461
-1.91322295487
-1.91322295493
-1.91322295494
-1.91322295494
-1.91322295498
-1.91322295498
-1.91322295498
-1.91322295498
Minimum:
(x, y) = [-0.54719755 -1.54719755]
```



### Newtons Method

```Python
import numpy as np
import math

def f(x, y):
	"""
	McCormick Function.
	MINIMUM: f(-0.54719755, -1.54719755) = -1.9132229549810367
	"""
	return math.sin(x + y) + (x - y) ** 2 - 1.5 * x + 2.5 * y + 1

"""
The partial derivative of the McCormick function with respect to x.
"""
def x_partial(a):
	return math.cos(a[0] + a[1]) + (2.0 * a[0]) - (2.0 * a[1]) - 1.5

"""
The partial derivative of the McCormick function with respect to y.
"""
def y_partial(a):
	return math.cos(a[0] + a[1]) - (2.0 * a[0]) + (2.0 * a[1]) + 2.5

def x_y_partial(a):
	return -2.0 - math.sin(a[0] + a[1])

def y_x_partial(a):
	return -2.0 - math.sin(a[0] + a[1])

def x_x_partial(a):
	return 2.0 - math.sin(a[0] + a[1])

def y_y_partial(a):
	return 2.0 - math.sin(a[0] + a[1]) 

# Hessian Matrix:
# [[ pL / pXpX , pL / pXpY ]
#  [ pL / pYpX , pL / pYpY ]]
def hessian(a):
	return np.array([[ x_x_partial(a), x_y_partial(a) ], 
					 [ y_x_partial(a), y_y_partial(a) ]]) 

"""
Utilizes Newton's method to find the minimum of the McCormick function.
Input:
	(1) a : NumPy array with two entries (the starting x and y)
"""
def nm_optimize(a):

	prev_theta = np.array([float('inf'), float('inf')]) # initially there is no previous theta so initialize to infinity
	cur_theta = a # initial guess for (x, y)

	while np.linalg.norm(cur_theta - prev_theta) > 10 ** -20:
		# save the theta from the last iteration
		prev_theta = cur_theta

		# at the end of each iteration, print out the value of the objective function
		print f(cur_theta[0], cur_theta[1])
		
		# calculate the inverse hessian and the gradient evaluated at the theta from the last iteration
		inv_hessian = np.linalg.inv(hessian(prev_theta))
		gradient = np.array([x_partial(prev_theta), y_partial(prev_theta)])
		
		# calculate a new theta from the previous theta
		cur_theta = prev_theta - np.dot(inv_hessian, gradient)

	print "Minimum:\n(x, y) =", cur_theta, "\n"
```



### Results

```
>> nm_optimize(np.array( [-0.2, -1.0] ))
-1.49203908597
-1.91281352075
-1.91322291866
-1.91322295498
-1.91322295498
-1.91322295498
Minimum:
(x, y) = [-0.54719755 -1.54719755]

>> nm_optimize(np.array( [-0.5, -1.5] )) 
-1.90929742683
-1.91322090085
-1.91322295498
-1.91322295498
-1.91322295498
Minimum:
(x, y) = [-0.54719755 -1.54719755]
```

