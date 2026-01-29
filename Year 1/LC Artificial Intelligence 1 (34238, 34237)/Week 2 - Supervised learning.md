---
date: 2026-01-29
updated: 2026-01-29T11:07:00
tags:
  - lessons/lecture
---
![[W2.1_preview.pdf]]
# Machine Learning
A **machine learning problem** is one that requires a **model** to be built from data. These include:
- classification
- estimation/prediction
### Forms of Machine Learning
There are three forms of Machine Learning (from the perspective of the *input*):
- [[#Supervised Learning]]
	- The most prevalent form
	- It requires a "*teacher*" which provides it with the expected output
	- Most problems are either **[[#Classification]]** or **[[#Regression]]** problems
	- eg: Spam detection, Stock price prediction
- Unsupervised Learning
	- Learning without a "*teacher*"
	- Useful to find hidden structures/insights in data
	- eg: Clustering
- Reinforcement Learning
	- Learning with delayed feedback
	- Learning from a a series of actions
	- eg: chess, robots, "*learning on the job*"

# Supervised Learning
### High Level Overview: 
Given some input $x$, try to predict appropriate output $y$. What is the function $f$ such that: 
$$ f(x) = y $$
### The Learning Process
1. Use examples of input-output pairs: **training data** $$(x^{(1)}, y^{(1)}),(x^{(2)}, y^{(2)}), \dots, (x^{(n)}, y^{(n)})$$
2. Supervised Learning is used to create a function $f$
3. Given a new input $x^k$, predict its output $y^k$
### Terminology
Vector notation ($i$-th input, $d$ dimensions):
$$x^{(i)} = (x_1^{i},x_2^{i},x_3^{i},\dots, x_d^{i})$$
**Input** = **Attribute(s)** = **Feature(s)** = **Independent variable(s)**
**Output** = **Target** = **Response** = **Dependent variable**
**Function** = **Hypothesis** = **Predictor**

## Classification
## Regression

## Overfitting and Underfitting
