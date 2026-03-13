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
A function that captures the trend between input and output. It outputs a continuous value, and is used to predict target values for new inputs.

> [!example]+ Example Regression Problem: Predicting weight from heigh
> ![[W2.2_preview.pdf#page=4&rect=40,29,466,273|W2.2_preview, p.4]]
> In this example, there is a clear trend between height and weight, therefore regression can be used to mathematically define this trend, allowing for predictions

### Univariate Linear Regression
There is one input attribute:
$$y = f(x; w_0, w_1) = w_1x + w_0$$

> [!tip] Parameters
> In this example, $x$ is the dependent variable; $w_0$ and $w_1$ are the free parameters, meaning they are the values in $f(x)$ that can be adjusted during learning to adjust the behaviour of the function.
> In the univariate linear case, $w_1$ would represent the gradient, and $w_0$ would represent the $y$-intercept of the regression line

The goal of univariate linear regression is to find the **line of best fit**. 





## Overfitting and Underfitting
	