---
tags:
  - "#FAA"
---

**Q1. Which of these is a reasonable definition of machine learning?**
**R:** ML is the field of study that gives computers the ability to learn without being explicitly programmed.

**Q2**
**The amount of rain that falls in a day is usually measured in either millimeters (mm) or inches. Suppose you use a**
**learning algorithm to predict how much rain will fall tomorrow. Would you treat this as a classification or a regression**
**problem?**
**R:** Regression. Predicting the amount of rain (measured in millimeters or inches) involves outputting a continuous real number, which defines it as a regression problem within supervised learning.
For classification the task would need to involve predicting a result that belongs to a fixed, discrete set of categories (Win or not win, binary outputs e.g.).

**Q3**
**Suppose you are working on stock market prediction. You would like to predict whether or not a certain company will**
**win a patent infringement lawsuit (by training on data of companies that had to defend against similar lawsuits).**
**Would you treat this as a classification or a regression problem?**
**R:** Classification. Predicting whether a company will win or lose (0/1) a lawsuit involves determining a categorical outcome, which defines it as a **classification** problem.

**Q4
Some of the problems below are best addressed using a supervised learning algorithm, and the others with an
unsupervised learning algorithm. To which of the following you would apply supervised learning? (Select all that
apply.) Determine if this is a classification or regression supervised learning.

**R:** 
-  **In farming, given data on crop yields over the last 50 years, learn to predict next year's crop yields.**
	**R:** Regression. This would be a supervised learning task of regression because it seeks to predict a real number (the quantity of crops yielded).
-  **Examine a web page, and classify whether the content on the web page should be considered "child-friendly" (e.g., non-pornographic, etc.) or "adult."**
	**R:** Classification. This would be a supervised learning task of classification because it assigns a categorical label to the input web page.
#### Extra (not needed)
-  **Given data on how 1000 medical patients respond to an experimental drug (such as effectiveness of the treatment, side effects, etc.), discover whether there are different categories or "types" of patients in terms of how they respond to the drug, and if so what these categories are.**
	**R:** Unsupervised Learning. The goal is to discover the internal structure of groupings within the dataset, rather than predicting a known output or label.
- **Given a large dataset of medical records from patients suffering from heart disease, try to learn whether there might be different clusters of such patients for which we might tailor separate treatments.**
	**R:** Unsupervised Learning. Clustering methods, such as K-means, are typical examples of unsupervised learning algorithms used to find relevant grouping in unlabeled data.

**Q5
Suppose that you have trained a logistic regression classifier, and it outputs on a new example x a prediction $h_{}θ(x)$=0.2. 
This means (check all that apply):**
**R:**  Our estimate for P(y=1|x;θ) is 0.2. and Our estimate for P(y=0|x;θ) is 0.8.
In logistic regression, the output of the hypothesis function $h_{\theta(x)}$ is interpreted as the estimated probability that the output class $y$ is equal to 1, given the input x and parameters $\theta$. Since the total probability must sum to 1 in this binary classification problem, the probability of the negative class ($y = 0$) is $1 - P(y=1|x;\theta)$ or $1-0.2=0.8$.

**Q6**
**Table 1 describes a simple example with two classes. Represent the data set in the space. Is this a linearly or**
**non-linearly separable problem?**
									![[faa_exercise_6.png]]
 
**R:** (-1,1); (1,-1); (-1,-1); (1,1)
This is a non-linearly separable problem. The data pattern show cannot be completely separated into two classes using a single straight line in the current input space.
![[Faa_resp_ex6.png]]

**Q7**
**Which of the following statements are true? Check all that apply.**

**R:** 
- Clustering is an example of unsupervised learning. 
- In unsupervised learning, the training set is of the form {$x^1, x^2,\dots, x^m$} without labels $y^i$
- In unsupervised learning, you are given an unlabeled dataset and are asked to find "structure" in the data.
Dimensionality reduction techniques like principal component Analysis (PCA) are also unsupervised learning approaches.

**Q8**
**Supposed you have the following training set, and fit a logistic regression classifier $h_\theta(x) = g(\theta_0 + \theta_1x_1 + \theta_2x_2)$.**
![[faa_ques8_image.png]]

**R:**
- J(θ) **will be a convex function, so gradient descent should converge to the global minimum**. The cost function $J(\theta)$ used in logistic regression is specifically chosen because it is a convex function. This property guarantees that the optimization algorithm, such as gradient descent, will always converge to the global minimum of the cost function, even if the data itself is not perfectly linearly separable.
- **Adding polynomial features (e.g., instead using** $hθ​(x)=g(θ_{0}+θ_{1}​x_{1}1+θ_{2}​x_{2}​+θ_{3}​x^{2}_{1}​+θ_{4}​x_{1}1x_{2}​+θ_{5}​x^2_{2}​)$ **could increase how well we can fit the training data.** Adding higher-order polynomial features maps the input data into a higher-dimensional space, which allows the logistic regression model to learn a more complex, non-linear decision boundary in the original $x_1-x_2$ plane. This increase in complexity typically allows the hypothesis to fit the observed training data points better, increasing training set performance.
#### Extra
**"Because the positive and negative examples cannot be separated using a straight line, linear regression will perform as well as logistic regression on this data"**
Linear Regression is generally unsuitable for classification tasks, especially binary classification where the labels are {0,1}. Linear regression models output continuous values ($h_{\theta}(x)$) is not bounded by 0 and 1, which makes interpreting the output as a probability problematic
 and Susceptible to outliers. Regression is specifically designed for classification using sigmoid function to bound outputs between 0 and 1, facilitating probabilistic interpretation and minimizing a specialized convex cost function. Even when data is not linearly separable, logistic regression is the theoretically superior choice over standard linear regression for classification problems.

**"The positive and negative examples cannot be separated using a straight line. So, gradient descent will fail to converge."**
The ability of the data to be linearly separable **does not** determine the convergence of Gradient Descent for Logistic Regression. This uses a **convex** cost function $J(\theta)$. Because the cost function is convex, gradient descent is mathematically guaranteed to converge to the global minimum, regardless of whether a perfect straight line can separate the training examples. If the data is not linearly separable, the minimum cost will simply be greater than zero, but convergence will still always occur.

**Q9**
**For logistic regression, the gradient is given by $\frac{∂}{∂θ_{}j}​J(θ)=\frac{1}{m}​\\sum^{m}_{i=1}=​(hθ​(x^i)−y^i)x^j_{i}​$. Which of these is a correct gradient descent update for logistic regression with a learning rate of α? Check all that apply.**
**R:**
- $θ:=θ−α\frac{1}{m}​ \sum^{m}_{i=1}(hθ​(x^i)−y^i)x^i$
The goal of the gradient descent update is to replace the parameter vector θ by moving it against the direction of the gradient.$$θ:=θ−α⋅∇J(θ) $$The expression above is the **vectorized form** of the gradient update rule of logistic regression. It uses the learning rate alpha to step away from the calculated gradient.

**Option 1** is incorrect because while mathematically equivalent if expanded, the general and common vectorized form uses $h_\theta(x^i)$. Also, the position of the $x^i$ outside the summation structure is incorrect.

**Option 2** is incorrect because it replaces the logistic regression hypothesis with the unsquashed linear prediction $\theta^T_{x}$. Using $\theta^T_{x}$ would be the gradient update for linear regression (mean squared error) not logistic regression (Log Loss).

**Option 4** Similar to option 2, this update uses the lienar prediction $\theta^T_{x}$ instead of the hupothesis. This error term is characteristic of Linear regression not of logic regression.

**Q10**
**Which of the following statements are true? Check all that apply**
**R:**
**Correct**
- **The cost function** J(θ) **for logistic regression trained with $m≥1$ examples is always greater than or equal to zero.**  The cost function for logistic regression is designed such that the cost so always non-negative.
- **The sigmoid function** $g(z)=\frac{1}{1+e^{-z}}$​ **is never greater than one** (>1). The sigmoid function is the hypothesis model for logistic regression, and its output is mathematically constrained to values between 0 and 1.

**Wrong**
- **For logistic regression, sometimes gradient descent will converge to a local minimum (and fail to find the global minimum). This is the reason we prefer more advanced optimization algorithms such as fminunc (conjugate gradient/BFGS /L-BFGS/etc).** The specialized cost function used in logistic regression (Log Ross) so mathematically designed to be a convex function, which always guarantees a convergence to the global minimum, not just a local minimum.
- **Linear regression always works well for classification if you classify by using a threshold on the prediction made by linear regression.** It's generally unsuitable for classification because the output is unbounded, can be bigger than 1 or lower than 0. Making probabilistic interpretation necessary for classification. This is the reason logistic regression uses the sigmoid function to overcome these limitations by bounding between 0 and 1. The question would be right if it was Logistic regression rather than linear.

**Q11**
**Supposed you train a logistic classifier $h_{\theta}(x) = g(\theta_{0} + \theta_{1}x_{1} + \theta_{2}x_{2})$. Suppose $\theta_{0} = - 6, \theta_{1} = 1, \theta_{2} = 0$. Which of the following figures represents the decision boundary found by your classifier?**

**R:** Correct choice is the second figure, vertical boundary at x=6 and y=1 on the right. The decision boundary is defined where the argument to the sigmoid is zero, $\theta_{0} + \theta_{1}x_{1} + \theta_{2}x_{2}=0$. Substituting the given values yields $-6 + 1x_1 + 0x_{2} = 0$, resulting in $x_1 = 6$. Since $\theta_{1}=1$, any point where $x_1 \textgreater 6$ results in a positive z and thus a prediction of y=1.

**Q12**
**You are training a classification model with logistic regression. Which of the following statements are true? Check all that apply.**
**R:** 
- **Adding a new feature to the model always results in equal or better performance on the training set.** Adding a feature generally provides the model with greater complexity and flexibility, enabling it to fit the current training data more closely.
- **Introducing regularization to the model always results in equal or better performance on examples not in the training set.** Regularization is explicitly used to combat overfitting, by penalizing large parameter values $\theta$, it aims to improve the model's ability to generalize to new, unseen examples not the ones in the training set or already seen.

**Wrong**
- **Adding many new features to the model helps prevent overfitting on the training set.** Adding new features typically increases the complexity of the model, which makes it more likely to fit noise and suffer from overfitting. That is why we use regularization or feature selection
- **Introducing regularization to the model always results in equal or better performance on the training set.** Regularization works by increasing the cost function by penalizing the magnitude of the $\theta$. This intentional constraint results in a model with higher bias and reduced variance, meaning that while it typically performs better on unseen data, and usually results in a worse fit on the training set.

**Q13 (lambda - the regularization parameter)**
**Suppose you ran logistic regression twice, once with $\lambda=0$, and once with $\lambda=1$. One of the times, you got parameters $\theta = \begin{matrix}74.81;44.05\end{matrix}$, and the other time you got $\theta = \begin{matrix}1.37;0.51\end{matrix}$. However, you forgot which value of $\lambda$ corresponds to which value of $\theta$. Which one do you think corresponds to $\lambda=1$?**
**R:** $\theta = \begin{matrix}1.37;0.51\end{matrix}$ 
Regularization works by adding a penalty term, proportional to $\lambda$, to the cost function to **shrink the model parameters** toward zero. When $\lambda = 0$, the model is optimized purely for fitting the training data without regularization, allowing the parameters to take on larger magnitudes if needed. When $\lambda=1$, regularization was applied and the optimization minimizes the prediction error and the magnitude of the parameters.

**Q14**
**Which of the following statements about regularization are true? Check all that apply.**
**R:** **Consider a classification problem. Adding regularization may cause your classifier to incorrectly classify some training examples (which it had correctly classified when not using regularization, i.e. when λ=0).** Regularization sacrifices fit to the training data to gain better generalization.
**Wrong**
- **Using too large a value of** λ **can cause your hypothesis to overfit the data; this can be avoided by reducing λ.** Increasing the $\lambda$ combats overfitting by forcing parameter magnitudes to decrease. Using large values of $\lambda$ leads to underfitting because the models becomes simple.
- **Because logistic regression outputs values $0≤hθ​(x)≤1$, its range of output values can only be "shrunk" slightly by regularization anyway, so regularization is generally not helpful for it.** Regularization directly penalizes the size of the $\theta$ not the direct output range of the sigmoid function. It's used to prevent overfitting when complex models or many features are used.
- **Using a very large value of** λ **cannot hurt the performance of your hypothesis; the only reason we do not set** λ **to be too large is to avoid numerical problems.** The bigger the value of lambda, the more severely it penalizes complexity of the model, leading to underfitting. This of course hurts the performance by increasing bias and can results in the model failing to properly learn the trend.

**Q15**
**In which one of the following figures do you think the hypothesis has overfit the training set?** 
**R:** The first figure, with the line changing course for each dot. This symbolizes the complexity of the model (overfitting), capturing noise and fitting every single training example perfectly.

**Q16**
**In which one of the following figures do you think the hypothesis has underfit the training set?**
**R:** The first figure. It shows a simple, gentle curve compared to the second figure which shows that this model is too simple (underfitting) and fails to capture the fundamental trend of the training data.

**Q17**
**The formula for the Gaussian Kernel is given by similarity $(x,l^1) = \exp(\frac{||x-l^1||^2}{2\sigma^2})$. The figure below shows a plot of $f_1 = similarity(x, l^1)$ when $\sigma^2 = 1$ **
Which of the following is a plot of $f_1$, when $\sigma^2 = 0.25$**
**R:** Picture of the left with a sharper, narrower peak. The parameter sigma^2 controls the spread of the Gaussian kernel. If sigma is decreased, the kernel function dictates that similarity value must drop off much faster as the distance increases. The results in a sharper, narrower peak centered at the landmark l^1. 

**Q18**
**Suppose you are trying to decide among a few different choices of kernel and are also choosing parameters such as C, $\sigma^2$, etc. How should you make the choice?**
**R:** Choose whatever performs best on the cross-validation data. To choose a hyperparameter (like kernel type) or to select the best model, the performance must be evaluated on the **Cross-validation** set.

**Q19**
**Suppose you run k-means using k=3 and k=5. You find that the cost function J is much higher for k=5 than for k=3. What can you conclude?**
**R:** **In the run with k=5, k-means got stuck in a bad local minimum. You should try re-running k-means with multiple random initializations.** The goal of K-means clustering algorithm is to minimize the distortion cost function J, which measure the average squared distance of each example to its assigned cluster centroid. 

**Q20**
**Which of the following is the recommended way to initialize k-means?**
**R:** ****Pick** k **distinct random integers** i1​,...,ik​ **from** {1,...,m}**. Set** μ1​=x(i1​),μ2​=x(i2​),...,μk​=x(ik​)**.***  The K-means algorithm is sensitive to its initial choice of cluster centroids and may converge to a local minimum. The recommended initialization strategy to avoid poor local optima is to select K initial cluster centroids that correspond directly to K **distinct training examples** chosen randomly from the dataset of **m** examples.

**Q21**
**Which of the following are good/recommended applications of PCA? Select all that apply.**
**R:** 
- **To reduce the dimension of the input data so as to speed up a learning algorithm.** Yes. Fewer dimensions often mean faster training and less computation.
- **To compress the data so it takes up less computer memory / disk space** Yes. PCA can reduce storage by keeping only the most important components.
- **To visualize high-dimensional data (by choosing k = 2 or k = 3)** Yes. PCA is commonly used for 2D or 3D visualization.

**Q22**
**You have the following neural network. You'd like to compute the activations of the hidden layer $\alpha^2 E R^3$. One way to do so is the following octave code:** 
![[FAA_Q22_octaveCode.png]]
**You want to have a vectorized implementation of this. Which of the following implementations correctly compute $\alpha_{2}$? Check all that apply.**
**R:** **a2 = sigmoid (Theta1 * x);** 
The goal is to compute the activations of layer 2 ($\alpha^2$) based on the input layer x (layer 1) and the parameter matrix $\theta$ labeled theta1 in the code. Based on the diagram, layer 1 has 3 inputs (the bias unit + 1, x1, and x2). If the input vector x includes the bias unit, its size is 3x1. The matrix $\theta$ maps these inputs to the 3 units in Layer2, and the code states its size is 3x3. 
The calculation for the weighted input $z^2$ before the activation function is performed using matrix multiplication: $$z^2 = \theta^{(1)}x$$
Dimensionally: $(3*3) * (3*1) = 3*1$

The final activation is achieved by applying the sigmoid function element-wise to the weighted sum: $$ a^2 = sigmoid(z^2) = sigmoid(\theta^1)x $$
The option a2= sigmoid ($\theta^1$ * x)

**Q23**
**Let $J(\theta) = 2*\theta^3 + 2$. Let $\theta=1$ and $e=0.01$. Use the formula $\frac{J(\theta+e) - J(\theta-e)}{2e}$ to numerically compute an approximation to the derivative at $\theta=1$. What value do you get? (when $\theta=1$, the true/exact derivative is $\frac{dJ(\theta)}{d\theta} = 6$.)**
**R:** $$\frac{J(1+0.01) - J(1-0.01)}{2*0.01}$$
$$\frac{(2*1.01^3 + 2) - (2*0.99^3 + 2)}{0.02}$$
$$\frac{ 4.060602 - 3.940598}{0.02} $$
$$ \frac{0.120004}{0.02}$$
$6.0002$ 

**Q24**
**Is it true that the KNN classifier needs the training set during the test phase? Justify your answer.**
**R:** k-NN is a non-parametric classifier that needs the training sett during the test phase. To classify a new record during the test phase, the algorithm must: compute the distance (similarity) from the new record to **all labeled records** in the training set. Identify the k nearest neighbors based on this calculated distance. Assign the class label of the new record based on the **majority label** of those k nearest neighbors. It relies entirely on the stored training data to make predictions, rather than learned parameters.

**Q25**
**Supposed you ran gradient descent three times, with different values for the parameter learning rate $\alpha=0.01, \alpha=0.1, \alpha=1$, and got the following three plots (A, B and C)**
**R:** **A is with α=0.1, B is with α=0.01, C is with α=1** Alpha dictates the step size taken during gradient descent as it attempts to minimize the cost function $J(\theta)$. Plot C diverges, which happens when learning rate is too large. Plot B is a slow convergence, which indicates a very small learning rate resulting in small steps. Plot A has an efficient convergence, which can only lead to the last value of alpha.

**Q26**
**Check all that apply regarding the typical characteristics of the back-propagation algorithm**
**R:** **It can be stacked into poor local minima.** 

#### Wrong
**It does not require labeled data.** It's listed under supervised learning, therefore requires labeled data.
**It is very slow in networks with multiple hidden layers.** The primary challenge noted for deep learning is the need for massive computational resources. I didn't find anything that could be of use in the slides to better answer why this is "wrong" It's just not in there, and therefore I'm not putting it has a correct answer.
**It cannot be applied to multilayer perceptron.*** Error backpropagation algorithm is the core learning process used to train **neural networks**. Multilayer perceptrons are a type of neural networks and are explicitly listed as an architecture related to deep learning.

**Q27**
**Which of the following ML architectures are related with deep learning? Check all that apply.**
**R:** Multilayer perceptron; Convolution Neural Network (CNN); **Sparse Stacked Autoencoder**(?)
#### Wrong
Reinforcement Learning is listed as a machine learning approach.

**Q28**
**Which of the following statements regarding Softmax Regression (SR) are true? Check all that apply.**
**R:** **Softmax Regression is a supervised learning algorithm**. Softmax Regression is used for classification problems, which fall under the category of **Supervised Learning**
**Softmax Regression is more suitable than Logistic Regression for mutually exclusive classes** Softmax Layers are designed to estimate the probability that an example belongs to each of the k classes. In contrast, **Logistic Regression** is natively a **binary classifier**, and when adapted for multiple classes (multiclass classification), it typically requires training separate models using the **One-versus-all** approach

#### Wrong
**Softmax Regression is a binary classifier:** This is false. Logistic regression is a binary classifier (outputting a probability between 0 and 1 for one class). The Softmax Layer is used to handle **multiple classes** (k outputs), estimating the probability for each class k
**The gradient descent cannot be applied for Softmax Regression:** This is false. Softmax Regression uses gradient-based learning methods to minimize its cost function. Gradient descent is a common iterative algorithm used to update parameters in classification models like Logistic Regression and Neural Networks (which often use a final Softmax layer)

**Q29. Suppose you have m=14 examples with n=3 features. What are the dimensions of the data matrix X, the output
y and the vector of parameters θ when you implement it .**
**R:** A. X is 14x4, y is 14x1,θ is 4x1
 m=14 corresponds to the number of rows in the data matrix X therefore X will have 14 rows and y will have 14 rows.
 
 The number of original features is n=3. When implementing models like linear or logistic regression, a constant feature ($x_{0}=1$) the **bias unit**, which is added to the feature vector x.
 
 The feature vector x for a single example is typically defined as a column vector with n+1 dimensions. (3+1)
 The data matrix X organizes these m examples as rows, meaning X will have m rows and n+1 columns. X = 14 x 4

**The output vector $y$(labels)** holds the label/target value for each of the m examples. Assuming a single target variable, $y$ is a column vector with m rows. $y:14*1$

**Parameter Vector $\theta$(weights)** must correspond to the number of columns (features) in data matrix X including the bias term $\theta_0$ $$(n+1)*1 = (3+1)*1 = 4 * 1$$
