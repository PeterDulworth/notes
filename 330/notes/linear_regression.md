# Linear Regression

https://www.youtube.com/watch?v=PPLop4L2eGk&list=PLLssT5z_DsK-h9vYZkQkYNWcItqhlRJLN

### Linear Regression with One Independant Variable

- Supervised - i.e. given the "right answer" for each example in data
- Regression - i.e. predict real-valued output

We use a linear hypothesis function $h_\theta(x) = \theta_0 + \theta_1 x$ to predict $y$ given $x$.

**Cost Function**

We want to learn $\theta_0, \theta_1$ so that the line fits our data well. i.e. choose $\theta_0, \theta_1$ so that $h_\theta(x)$ is cose to $y$ for our training examples $(x, y)$.

To do this we want to choose the $\theta_0, \theta_1$ that minimize the square error of the difference between our hypothesis function and the actual result. i.e. we want to choose thetas to our cost function $J$:
$$
J(\theta)=\frac{1}{2m}\sum_{i=1}^{m}(h_\theta(x_i)-y_i)^2
$$
We can simplify everything by dropping $\theta_0$ (setting equal to 0). Cost function should be a function of whatever you are trying to learn. We then want to find the theta that gives us the lowest value of the cost function.

In this context, the cost function basically describes the "goodness" of a given line of best fit over an entire data set.

**Minimizing The Cost Function Using Gradient Descent**

Have $J(\theta_0, \theta_1)$. Want $\min_{\theta_0, \theta_1}J(\theta_0,\theta_1)$. Outline:

- Start with some $\theta_0, \theta_1$
- Keep changing $\theta_0, \theta_1$ to reduce $J(\theta_0, \theta_1)$ until we end up at a minimum 

### Multiple Linear Regression

Same as linear regression with on variable but now we have multiple features.

# Logistic Regression

https://www.youtube.com/watch?v=-la3q9d7AKQ&list=PLLssT5z_DsK-h9vYZkQkYNWcItqhlRJLN&index=32

**Classification**

- Email: Spam / Not Spam?
- Online Transactions: Fradulent (Yes / No)?
- Tumor: Malignant / Benign?

Binary Classification:
$$
y \in {0, 1}
$$
0: "Negative Class"

1: "Positive Class"

Can generalize to multi classification: $y\in\{0, 1,...,k\}$

Hypothesis function: $h_\theta(x)=g(\theta^Tx)$ where $g(z)=\frac{1}{1+e^{-z}}$. i.e.
$$
h_\theta(x)=\frac{1}{1+e^{-\theta^Tx}}
$$
$g(z)$ is the sigmoid (logistic) function.

$g(z)\geq0.5$ when $z \geq 0.5$. i.e. $h_\theta(x)\geq0.5$ whenever $\theta^Tx \geq 0.5$

