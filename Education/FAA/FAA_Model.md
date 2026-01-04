#FAA 

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
The ability of the data to be linearly seperable **does not** determine the convergence of Gradient Descent for Logistic Regression. This uses a **convex** cost function $J(\theta)$. Because the cost function is convex, gradient Descent