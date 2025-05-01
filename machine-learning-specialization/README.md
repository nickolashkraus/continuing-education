# Machine Learning Specialization

## Week 1: Introduction to Machine Learning

### Overview of Machine Learning

**What is machine learning?**

In 1959, Arthur Samuel described machine learning as the "field of study that gives computers the ability to learn without being explicitly programmed".

### Supervised vs. Unsupervised Machine Learning

With supervised learning the model is trained using *labeled* data, that is, input *x* has output *y*. Common applications include spam filtering, prediction, and classification.

Unsupervised learning, on the other hand, is trained using *unlabeled* data, meaning for a given input the model is not provided a corresponding output. Examples include segmentation (or clustering), anomaly detection, and dimensionality reduction.

### Regression Model

#### Linear Regression

**What is linear regression?**

Linear regression is a fundamental statistical and machine learning technique that models the relationship between a dependent variable (target) and one or more independent variables (features) by fitting a linear equation to the observed data.

The simplest form is simple linear regression, which can be expressed as `y = mx + b`.

where:
  * `y` is the dependent variable (what we're trying to predict)
  * `x` is the independent variable (our input feature)
  * `m` is the slope (coefficient)
  * `b` is the y-intercept

For multiple features, we use multiple linear regression: `y = b₀ + b₁x₁ + b₂x₂ + ... + bₙxₙ`

The model finds the best-fit line by minimizing the sum of squared differences between predicted and actual values (called the "least squares" method).

When applied to machine learning, the training set represents the independent variables (features). `y` represents the learning algorithm (`f(x,y)`), which can be used to produce estimates (`ŷ`).

#### Cost Function

**What is a cost function?**

A cost function (also called a loss function) is a measure of how well a machine learning model is performing. It calculates the difference between the model's predictions and the actual target values, essentially quantifying the "cost" or "penalty" of making incorrect predictions.

The canonical cost function for a linear regression model is call the **Mean Squared Error (MSE)**:

$$
J(w,b) = \frac{1}{2m} \sum_{i=1}^m (ŷ^{(i)} - y^{(i)})^2
$$

where,
* `J(w,b)` is the cost function
* `m` is the number of training examples
* `ŷ` is the predicted value
* `y` is the actual value


The goal of training a machine learning model is to minimize this cost function `J(w,b)`-in other words, to find the model parameters (`w` and `b`) that make the predictions as close as possible to the actual values. `w` and `b` are commonly referred to as "coefficients" or "weights".

## Gradient Descent

**What is a gradient descent?**

Gradient descent is an optimization algorithm used to find the minimum of a cost function.

$$
\theta_{j+1} = \theta_j - \alpha \frac{\partial}{\partial \theta_j} J(\theta)
$$

where,
* `θ` represents the parameters
* `α` is the learning rate
* `J(θ)` is the cost function

Parameters are repeated calcuated simultaneously until convergence.

## Week 2: Regression with multiple input variables

### Vectorization

**What is a vectorization?**

Vectorization in machine learning refers to the process of converting operations that would otherwise be performed using loops into operations on vectors or matrices. This approach leverages specialized hardware, such as SIMD (Single Instruction, Multiple Data) units and GPUs and optimized libraries (ex. NumPy) to perform calculations in parallel rather than sequentially.

**What is a multiple linear regression?**

Multiple linear regression is a statistical method that models the relationship between a dependent variable (target) and two or more independent variables (features). It extends simple linear regression, which uses only one independent variable.

The cost function for multiple linear regression:

$$
J(\mathbf{w},b) = \frac{1}{2m} \sum\limits_{i = 0}^{m-1} (f_{\mathbf{w},b}(\mathbf{x}^{(i)}) - y^{(i)})^2
$$

where:

$$
f_{\mathbf{w},b}(\mathbf{x}^{(i)}) = \mathbf{w} \cdot \mathbf{x}^{(i)} + b
$$

`f` can be calcuated using Python with NumPy's `dot()` function, enabling parallelized calculations:

```python
f = np.dot(w, x) + b
```

The gradient descent update formulas for multiple linear regression are:

$$
\begin{align*}
\text{repeat}&\text{ until convergence:} \; \lbrace \\
& w_j = w_j -  \alpha \frac{\partial J(\mathbf{w},b)}{\partial w_j} \; & \text{for } j = 0,1,...,n-1\\
& b = b -  \alpha \frac{\partial J(\mathbf{w},b)}{\partial b}  \\
\rbrace
\end{align*}
$$

Where the partial derivatives are given by:

$$
\begin{align}
\frac{\partial J(\mathbf{w},b)}{\partial w_j} &= \frac{1}{m} \sum\limits_{i = 0}^{m-1} (f_{\mathbf{w},b}(\mathbf{x}^{(i)}) - y^{(i)})x_{j}^{(i)} \\
\frac{\partial J(\mathbf{w},b)}{\partial b} &= \frac{1}{m} \sum\limits_{i = 0}^{m-1} (f_{\mathbf{w},b}(\mathbf{x}^{(i)}) - y^{(i)})
\end{align}
$$

**Where:**
- $n$ is the number of features
- $m$ is the number of training examples
- $\alpha$ is the learning rate
- $f_{\mathbf{w},b}(\mathbf{x}^{(i)})$ is the model's prediction for example $i$
- $y^{(i)}$ is the actual target value for example $i$
- $x_{j}^{(i)}$ is the value of feature $j$ for example $i$

Note that all parameters $w_j$ and $b$ are updated simultaneously in each iteration.

### Feature Scaling

**What is a feature scaling?**

### Feature Engineering

### Polynomial Regression
