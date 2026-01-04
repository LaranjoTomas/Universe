**Machine Learning Fundamentals (L1)**

Machine learning (ML) is the field of study that gives computers the ability to learn without being explicitly programmed. A program learns if its performance on a task (T), as measured by performance (P), improves with experience (E). ML and Deep Learning (DL) form the core of Artificial Intelligence (AI).

|                                 |                                                                                                              |                                                                 |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------- |
| Learning Type                   | Characteristics                                                                                              | Examples/Algorithms                                             |
| **Supervised Learning**         | Uses labeled data (examples with "correct answers").                                                         | Linear/Logistic Regression, NN, SVM, Decision Tree, k-NN [3].   |
| _Regression_                    | Labels are real numbers (e.g., predicting house price, rain accumulation) [5, 6].                            | Linear Regression, Multivariate Regression [3].                 |
| _Classification_                | Labels are categorical values (e.g., spam/not spam, win/lose lawsuit) [5, 7].                                | Logistic Regression, SVM, Naive Bayes, k-NN [3].                |
| **Unsupervised Learning**       | Uses unlabeled data to discover hidden internal structure [4, 8].                                            | K-means clustering, Principal Component Analysis (PCA) [9, 10]. |
| **Deep Learning (DL)**          | Uses Deep Neural Networks (DNN) to automatically extract hidden features (representation learning) [11, 12]. | CNN, LSTM [9, 13].                                              |
| **Reinforcement Learning (RL)** | On-line learning through trial-and-error, maximizing expected rewards [4, 14].                               | Intelligent robotics, autonomous systems [4].                   |

**Data Handling and Visualization (L1)** Data features can be **Numeric** (floats, integers), **Boolean**, or **Categorical** (e.g., colors, gender) [15]. Categorical features are often processed using **One-hot encoding** [15]. Visualization tools include **Histograms** (showing distribution of a single feature), **Box Plots** (displaying data distribution using quartiles/outliers), and **Scatter Plot Arrays** [16, 17].

### **Linear and Logistic Regression (L2, L3)**

#### **Linear Regression (L2)**

• **Hypothesis:** $h_{\theta}(x) = \theta ^T x$ (a linear model).

• **Cost Function:** Mean Squared Error (MSE) $J(\theta)= \frac{1}{2m} \\sum^{m}_{i=1} (h(\theta)(x^i)-y^i)^2$.

• **Optimization:** **Gradient Descent** (Batch, Mini-batch, Stochastic) updates parameters $\theta_j$ iteratively in the direction of steepest descent.

   ◦ The **learning rate ($\alpha$)** controls the step size; if too small, convergence is slow; if too large, it may fail to converge (diverge).

#### **Overfitting and Regularization (L2, L3, L6)**

• **Overfitting (High Variance):** Model fits training data too well (complex, typically using many features or high-order polynomial terms) but fails to generalize to new data.

   ◦ _Remedy:_ Get more training examples, try smaller sets of features, or increase the regularization parameter lambda.

• **Underfitting (High Bias):** Model is too simple and fails to capture the underlying trend (e.g., low-order polynomial model).

   ◦ _Remedy:_ Get additional features, try adding polynomial features, or decrease the regularization parameter lambda.

• **Regularization:** Adds a penalty term to the cost function to shrink model parameters $\theta$ toward zero, reducing the variance.

   ◦ **Ridge Regression (L2 norm):** Reduces the magnitude of $\theta$ but keeps all features.

   ◦ **Lasso Regression (L1 norm):** Can shrink some coefficients of $\theta$ exactly to zero, serving as a feature selection tool.

   ◦ A very large value of lambda can lead to **underfitting**.

**Logistic Regression (L3)**

• **Model (Hypothesis):** Uses the **sigmoid function** $g(z)=\frac{1}{(1+e−z)}$ to constrain output between 0 and 1, interpreting it as the probability $P(y=1∣x;\theta)$.

• **Cost Function:** **Binary Cross-Entropy** or **Log Loss** is used to ensure the cost function J(theta) is convex, facilitating efficient optimization.

• **Decision Boundary:** Defined by thetaTx=0 (for a decision threshold of 0.5).

• **Nonlinearly Separable Data:** Use polynomial features (e.g., x_12,x_1x_2) to map the original space to a higher-dimensional feature space where data may be linearly separable.

• **Multiclass Classification:** Handled using the **One-versus-all** strategy, training a binary classifier for each class.

**Support Vector Machines (SVM) (L5)**

• **Objective:** SVM is a **Large Margin Classifier** that seeks a decision boundary (**hyperplane**) maximizing the distance (**margin**) to the closest training examples (**support vectors**).

• **Cost Function:** A modified version of the regularized logistic regression cost, optimized to minimize classification error and maximize the margin.

   ◦ Hyperparameter C controls the penalty for misclassified training examples (equivalent to $\frac{1}{\lambda}$); Large C means fitting training data closely (low bias, high variance); Small C means prioritizing larger margin (high bias, low variance).

• **Kernel Trick (Nonlinear SVM):** Used to classify nonlinearly separable data by implicitly mapping the input space into a higher-dimensional feature space where a linear separation is possible.

   ◦ **Gaussian Radial Basis Function (RBF) Kernel** is commonly used, acting as a similarity metric.

   ◦ Hyperparameter sigma (or gamma=1/sigma) controls the kernel spread. Small sigma leads to less smooth features (low bias, high variance); Large sigma leads to smoother features (high bias, low variance).

**Model Evaluation and Validation (L5, L6)**

• **Data Split:** Datasets should be split into **Training set** (for learning parameters), **Cross Validation (CV)/Development (Dev) set** (for selecting model and hyperparameters), and **Test set** (for final unbiased evaluation).

• **K-Fold Cross Validation:** Divides the training data into K subsets (folds), using K−1 for training and the remaining one for validation iteratively. The final validation error is averaged over K trainings.

• **Bias vs. Variance Diagnosis (L6):**

   ◦ **High Bias (Underfitting):** Training error (E_train) and CV error (E_CV) are both high [25].

   ◦ **High Variance (Overfitting):** E_train is low, but E_CV is significantly higher [25].

• **Performance Metrics:** Based on the **Confusion Matrix** (True Positives, False Negatives, etc.) [44].

    ◦ **Accuracy** is the fraction of correctly classified examples, but it is misleading for **unbalanced data sets** [45, 46].

    ◦ Other metrics include **Precision, Recall (True Positive Rate/Sensitivity)**, **Specificity (True Negative Rate)**, **F1 Score** (harmonic mean of Precision and Recall), and **AUC** (Area Under the ROC Curve) [47-49].

    ◦ Solutions for **Class Imbalance** include re-sampling methods or using weighted loss functions [46].

**Other Supervised Algorithms (L5, L7)**

• **k-Nearest Neighbor (k-NN) Classifier (L5):** A non-parametric classifier. To classify a new record, it computes the distance to all labeled records in the training set (which must be kept). The class assigned is the majority label of its K nearest neighbors [50, 51].

• **Naïve Bayes (NB) Classifier (L7):** A probabilistic classifier that uses Bayes Theorem to determine the probability of a new example belonging to each class [52, 53].

    ◦ Its main assumption is that all **features are independent** [54].

    ◦ It uses **Laplace correction** if the likelihood of a feature value is zero, preventing the entire probability product from becoming zero [54, 55].

• **Decision Tree (DT) Classifier (L7):** Classifies data by splitting nodes based on features. The best feature split is chosen by maximizing **Information Gain** (or Gain Ratio), typically measured by minimizing node **Entropy** or **Gini Index** (measures of node impurity) [56, 57]. **Pruning** (post-pruning preferred) is used to prevent overfitting [58].

**Unsupervised Learning Algorithms (L8)**

• **K-Means Clustering:** Iterative algorithm where data points are assigned to K cluster centroids based on distance (e.g., Euclidean distance), and then centroids are recomputed as the mean of their assigned points [59].

    ◦ The objective is to minimize **distortion** (average squared distance) [59].

    ◦ The algorithm is sensitive to initialization and may converge to a **local minimum**, thus requiring multiple random initializations [60].

    ◦ The **Elbow method** helps estimate the optimal number of clusters K [60].

• **Principal Component Analysis (PCA):** A dimensionality reduction technique used for data compression, speeding up algorithms, and visualization [61].

    ◦ PCA identifies the orthogonal directions (principal components) of maximum variance to project the original data (dimension n) onto a lower space (dimension k) [62, 63].

    ◦ Requires data preprocessing (mean normalization) [64].

    ◦ k is typically chosen such that a large percentage of variance is retained (e.g., 99%) [63].

**Advanced ML/DL Topics (L9, L10, L11, L12)**

• **Anomaly Detection (L9):** Used when positive examples (anomalies) are rare [65].

    ◦ **Method:** Fit a probability distribution p(x) to the normal (non-anomalous) training data [66]. A new example is flagged as an anomaly if $p(x\_{test}) < \\epsilon$ (a chosen threshold) [66, 67].

    ◦ **Univariate Gaussian:** Assumes feature independence. Computationally cheaper [68].

    ◦ **Multivariate Gaussian:** Uses a covariance matrix (Sigma) to capture correlation between features. Used when features are known to be highly correlated [69, 70]. Requires many more training examples (m) than features (n) for matrix inversion [71].

    ◦ **Evaluation:** Due to imbalance, metrics like F1-score or Precision/Recall are used to select epsilon [72].

• **Convolutional Neural Networks (CNN) (L10):** Specialized for processing high-dimensional inputs like images.

    ◦ **Convolution Operation:** Filters (kernels) learn features (like edges) through shared parameters across the image, dramatically reducing the number of total parameters compared to fully connected layers, thus mitigating overfitting [73-75].

    ◦ **Pooling Layer (Max/Average):** Reduces representation size and makes features more robust (no parameters to learn) [74, 76].

    ◦ **Transfer Learning:** Reusing parameters from a pre-trained model (e.g., trained on 1000 classes) and fine-tuning only the final classification layer for a new, smaller dataset [77].

    ◦ **Data Augmentation:** Techniques like rotation, noise addition, or cropping are used to artificially expand the dataset [78].

• **Recurrent Neural Networks (RNN) and Time Series (L11):** Designed for sequence models (variable input/output lengths) [79, 80].

    ◦ **Functionality:** Reads sequences step-by-step, passing activation states (alangletrangle) forward to provide context [80].

    ◦ **Training:** Uses **Backpropagation Through Time (BPTT)** [81].

    ◦ **Limitation:** Standard RNN suffers from **vanishing gradients**, resulting in short-term memory [82].

    ◦ **Solutions:** **Long Short-Term Memory (LSTM)** and **Gated Recurrent Units (GRU)** use internal mechanisms (memory cell c and gates) to capture long-range dependencies [83, 84].

• **Recommender Systems (L12):** Predict ratings/preferences [85].

    ◦ **Content-Based:** Uses known item features (x(i)) and user parameters (theta(j)) to predict ratings via linear regression [86].

    ◦ **Collaborative Filtering:** Based on the idea that similar users like similar things [87]. It simultaneously learns both the latent item features X and user parameters theta by minimizing a single cost function [88]. **Mean normalization** is often applied as a preprocessing step [89].

--------------------------------------------------------------------------------

Part 2: Answers to FAA_Final_exam_model.pdf Questions

**Q1. Which of these is a reasonable definition of machine learning?**o **ML is the field of study that gives computers the ability to learn without being explicitly programmed.** [1, 90]

**Q2 The amount of rain that falls in a day... Would you treat this as a classification or a regression problem?**o **Regression** [5, 91]

**Q3 Suppose you are working on stock market prediction. You would like to predict whether or not a certain company will win a patent infringement lawsuit... Would you treat this as a classification or a regression problem?**o **Classification** [5, 91]

**Q4 Some of the problems below are best addressed using a supervised learning algorithm... (Select all that apply.) Determine if this is a classification or regression supervised learning.**o Examine a web page, and classify whether the content on the web page should be considered "child friendly" (e.g., non-pornographic, etc.) or "adult." (Classification) [5, 92] o In farming, given data on crop yields over the last 50 years, learn to predict next year's crop yields. (Regression) [5, 92]

**Q4 (continued) Which of the following statements are true? Check all that apply.**square **In unsupervised learning, the training set is of the form** x(1),x(2),...,x(m) **without labels** y(i)**.** [8, 92] square **Clustering is an example of unsupervised learning.** [14, 92] square **In unsupervised learning, you are given an unlabeled dataset and are asked to find "structure" in the data.** [8, 92]

**Q5 Suppose that you have trained a logistic regression classifier, and it outputs on a new example x a prediction h_theta(x)=0.2. This means (check all that apply):**o **Our estimate for** P(y=1∣x;theta) **is 0.2.** [93] o **Our estimate for** P(y=0∣x;theta) **is 0.8.** [93]

**Q6 Table 1 describes simple example with two classes. Represent the data set in the space. Is this a linearly or nonlinearly separable problem?** *Based on the inputs (pm1,pm1) and outputs (pm1), this pattern is equivalent to an XOR function.*o **Nonlinearly separable problem** [24]

**Q7 Suppose you have the following training set, and fit a logistic regression classifier... Which of the following are true? Check all that apply.**square J(theta) **will be a convex function, so gradient descent should converge to the global minimum.** [30, 93] square **Adding polynomial features (e.g., instead using** h_theta(x)=g(theta_0+theta_1x_1+theta_2x_2+theta_3x_12+theta_4x_1x_2+theta_5x_22) **could increase how well we can fit the training data.** [33, 93]

**Q8 For logistic regression, the gradient is given by... Which of these is a correct gradient descent update for logistic regression with a learning rate of** alpha**? Check all that apply.**square theta:=theta−alphafrac1msum_i=1m(h_theta(x(i))−y(i))x(i). [31, 94]

**Q8 (continued) Which of the following statements are true? Check all that apply.**square **The cost function** J(theta) **for logistic regression trained with** mge1 **examples is always greater than or equal to zero.** [30, 31, 94] square **The sigmoid function** g(z)=frac11+e−z **is never greater than one** $(> 1)$**.** [29, 94]

**Q9 Suppose you train a logistic classifier** h_theta(x)=g(theta_0+theta_1x_1+theta_2x_2)**. Suppose** theta_0=−6,theta_1=1,theta_2=0**. Which of the following figures represents the decision boundary found by your classifier?** *The decision boundary is found where thetaTx=0, so −6+1x_1=0, leading to x_1=6. For $x\_1 > 6$, y=1.*o **Figure: (The image showing a vertical boundary at** x_1=6 **with** y=1 **area on the right.)** [32, 94]

**Q10 You are training a classification model with logistic regression. Which of the following statements are true? Check all that apply.**square **Adding a new feature to the model always results in equal or better performance on the training set.** [92] square **Introducing regularization to the model always results in equal or better performance on examples not in the training set.** [26, 92]

**Q11 Suppose you ran logistic regression twice, once with** lambda=0**, and once with** lambda=1**. Which one do you think corresponds to** lambda=1**?** *Regularization (lambda=1) shrinks parameters toward zero.*o theta=beginbmatrix1.370.51endbmatrix [93]

**Q12 Which of the following statements about regularization are true? Check all that apply.**square **Consider a classification problem. Adding regularization may cause your classifier to incorrectly classify some training examples (which it had correctly classified when not using regularization, i.e. when** lambda=0**).** [93, 95]

**Q13 In which one of the following figures do you think the hypothesis has overfit the training set?** *Overfitting is indicated by a complex curve fitting all training points exactly.*o **Figure: (The first figure showing a highly complex, wiggly curve passing exactly through the points.)** [23, 94]

**Q14 In which one of the following figures do you think the hypothesis has underfit the training set?** *Underfitting is indicated by a very simple model failing to capture the underlying data trend.*o **Figure: (The second figure showing a simple straight line that misses most points.)** [23, 94]

**Q15 The formula for the Gaussian kernel is given by... Which of the following is a plot of** f_1 **when** sigma2=0.25**?** *A smaller sigma2 (0.25 vs 1) leads to a narrower, sharper peak because the similarity decreases less smoothly with distance.*o **Figure: (The figure showing a sharper peak than the original image.)** [38, 96]

**Q16 Suppose you are trying to decide among a few different choices of kernel and are also choosing parameters such as C,sigma2, etc. How should you make the choice?**o **Choose whatever performs best on the cross-validation data.** [42, 97]

**Q17 Suppose you run k-means using** k=3 **and** k=5**. You find that the cost function** J **is much higher for** k=5 **than for** k=3**. What can you conclude?** *Since increasing K should generally decrease or keep J constant, a high J suggests a bad local minimum.*o **In the run with** k=5**, k-means got stuck in a bad local minimum. You should try re-running k-means with multiple random initializations.** [60, 97]

**Q18 Which of the following is the recommended way to initialize k-means?**o **Pick** k **distinct random integers** i_1,...,i_k **from** 1,...,m**. Set** mu_1=x(i_1),mu_2=x(i_2),...,mu_k=x(i_k)**.** [60, 97]

**Q19 Which of the following are good/recommended applications of PCA? Select all that apply.**square **To compress the data so it takes up less computer memory / disk space** [61, 98] square **To reduce the dimension of the input data so as to speed up a learning algorithm** [61, 98] square **To visualize high-dimensional data (by choosing** k=2 **or** k=3**)** [61, 98]

**Q20 You have the following neural network... You want to have a vectorized implementation of this... Which of the following implementations correctly compute** a(2)**? Check all that apply.** _Assuming_ x _includes the bias unit_ x_0_, and_ Theta1 _is the parameter matrix._square **a2 = sigmoid (Theta1 * x);** [98, 99]

**Q21 Let** J(theta)=2theta3+2**. Let** theta=1**, and** epsilon=0.01**. Use the formula... What value do you get?** *Calculation: (J(1.01)−J(0.99))/0.02=((2(1.01)3+2)−(2(0.99)3+2))/0.02=6.0002*o **6.0002** [98]

**Q22 Is it true that the KNN classifier needs the training set during the test phase? Justify your answer.** **True.** The k-Nearest Neighbor (k-NN) classifier needs the training set during the test phase [50]. This is because the algorithm classifies a new example by computing its distance to all existing labeled training examples to identify the k nearest neighbors and determine the class based on majority vote [51].

**Q23 Suppose you ran gradient descent three times, with different values for the parameter learning rate** alpha=0.01,alpha=0.1**, and** alpha=1**, and got the following three plots... Which plots corresponds to which values of** alpha**?** *A (fast convergence), B (slow convergence), C (divergence).*o **A is with** alpha=0.1**, B is with** alpha=0.01**, C is with** alpha=1**.** [21, 22, 96]

**Q26: Check all that apply regarding the typical characteristics of the back-propagation algorithm.**square **It can be stacked into poor local minima.** [30, 96]

**Q27: Which of the following ML architectures are related with deep learning? Check all that apply.**square **Sparse Stacked Autoencoder** [97] square **Multilayer perceptron.** [97] square **Convolution Neural Network (CNN)** [9, 97]

**Q28: Which of the following statements regarding Softmax Regression (SR) are true? Check all that apply.**square **Softmax Regression is a supervised learning algorithm** [3, 97] square **Softmax Regression is more suitable than Logistic Regression for mutually exclusive classes.** [98]

**Q29. Suppose you have** m=14 **examples with** n=3 **features. What are the dimensions of the data matrix** X**, the output** y **and the vector of parameters** theta **when you implement it?** *Assuming standard inclusion of the bias unit.*o **A.** X **is** 14times4**,** y **is** 14times1**,** theta **is** 4times1 [18, 98]

**Q30. Propose a solution to treat the problem of class unbalanced data.**Solutions to treat class unbalanced data include using **re-sampling methods** (such as oversampling or undersampling the minority class) or applying a **Weighted Binary Cross Entropy Loss** function to give more importance to the minority class examples [45, 46].