# Supervised Learning

- most classic problem in machine learning

given a bunch of $(x_i, y_i)$ pairs learn how to predict $y$  from $x$

$x$ is a set of features. $y$ is features we are trying to predict.

e.x. given medical records label with "breast cancer or not". would be supervised if we had a set of correctly labeled training records.

two sub problems: (depends on nature of $y$)

- classification: discrete set of labels that we want to apply (i.e. $y\in$ {yes, no})
- regression: outcome to predict is a real number $y\in\mathbb{R}$

there are lots of other sub problems / catagories but these are the biggest two.

### What models are used?

Many!

deep learning is generally supervised

most common: linear regression

from $x_i$ predict $y_i$ 

### How do you score a model?

simplest: **% correct (accuracy)**

- pros: simple
- cons: doesn't differentiate between all the ways you can be wrong: false positives, false negatives. too course grained, not realistic
- e.g. breast cancer in men. say 1 in 1000 men get breast cancer. if you have a classifier that takes any man and says NO you don't have breast cancer that classifier is %99.9 accurate but its a useless classifier.

**false positive / false negative rates:**

- false negative rate = (num we say are false that are rally true) / num that are true
- false positive rate = (num we say are true that are rally false) / num that are false

- breast cancer example: say we have a large data set in which 100 men have breast cancer but our model classifies all men as not having breast cancer. the our false negative rate is 100/100 so our model gets punished.

almost equivalent **recall and precision:**

- recall: num we say are true that are actually true / num that are true
- precision: num we say are true that are actually true / num we say are true 
- cons: 2 numbers instead of 1

**F1** : puts precision and recall into a single number
$$
F_1 = \frac{2 \times precision \times recall}{precision + recall}
$$
gives large score if you get both of these things together

