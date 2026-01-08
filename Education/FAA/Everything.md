---
tags:
  - "#FAA"
---

**Machine Learning Fundamentals (L1)**

Machine learning (ML) is the field of study that gives computers the ability to learn without being explicitly programmed. A program learns if its performance on a task (T), as measured by performance (P), improves with experience (E). ML and Deep Learning (DL) form the core of Artificial Intelligence (AI).

|                                 |                                                                                                     |                                                            |
| ------------------------------- | --------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| Learning Type                   | Characteristics                                                                                     | Examples/Algorithms                                        |
| **Supervised Learning**         | Uses labeled data (examples with "correct answers").                                                | Linear/Logistic Regression, NN, SVM, Decision Tree, k-NN   |
| _Regression_                    | Labels are real numbers (e.g., predicting house price, rain accumulation)                           | Linear Regression, Multivariate Regression                 |
| _Classification_                | Labels are categorical values (e.g., spam/not spam, win/lose lawsuit).                              | Logistic Regression, SVM, Naive Bayes, k-NN                |
| **Unsupervised Learning**       | Uses unlabeled data to discover hidden internal structure.                                          | K-means clustering, Principal Component Analysis (PCA)10]. |
| **Deep Learning (DL)**          | Uses Deep Neural Networks (DNN) to automatically extract hidden features (representation learning). | CNN, LSTM                                                  |
| **Reinforcement Learning (RL)** | On-line learning through trial-and-error, maximizing expected rewards.                              | Intelligent robotics, autonomous systems                   |

**Data Handling and Visualization (L1)** Data features can be **Numeric** (floats, integers), **Boolean**, or **Categorical** (e.g., colors, gender). Categorical features are often processed using **One-hot encoding**. Visualization tools include **Histograms** (showing distribution of a single feature), **Box Plots** (displaying data distribution using quartiles/outliers), and **Scatter Plot Arrays**.

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

• **Cost Function:** **Binary Cross-Entropy** or **Log Loss** is used to ensure the cost function $J(\theta)$ is convex, facilitating efficient optimization.

• **Decision Boundary:** Defined by $\theta_{T_{x}}$= 0 (for a decision threshold of 0.5).

• **Non-linearly Separable Data:** Use polynomial features (e.g., $x_{12}$,$x_{1x_{2}}$) to map the original space to a higher-dimensional feature space where data may be linearly separable.

• **Multiclass Classification:** Handled using the **One-versus-all** strategy, training a binary classifier for each class.

------------------------------------------

**One-versus-all strategy** is a technique used for **multiclass classification** problems, particularly when using binary classifiers like Logistic Regression. 
**Training Process:** For each class c from 1 to K:
    ◦ A specific binary output variable, $y_{binary}​$, is created
    ◦ Examples belonging to class c are labeled as $y_{binary}​$=1
    ◦ Examples belonging to all other classes are grouped together and labeled as $y_{binary}​$​=0
    ◦ A classifier is trained using the training data X and this new binary output $y_{binary}​$​
    ◦ The learned parameters ($\theta$) for this classifier are saved, typically in one matrix indexed by class c
    
-----------------------

**Support Vector Machines (SVM) (L5)**

• **Objective:** SVM is a **Large Margin Classifier** that seeks a decision boundary (**hyperplane**) maximizing the distance (**margin**) to the closest training examples (**support vectors**).

• **Cost Function:** A modified version of the regularized logistic regression cost, optimized to minimize classification error and maximize the margin.

   ◦ Hyperparameter C controls the penalty for misclassified training examples (equivalent to $\frac{1}{\lambda}$); Large C means fitting training data closely (low bias, high variance); Small C means prioritizing larger margin (high bias, low variance).

• **Kernel Trick (Nonlinear SVM):** Used to classify non-linearly separable data by implicitly mapping the input space into a higher-dimensional feature space where a linear separation is possible.

   ◦ **Gaussian Radial Basis Function (RBF) Kernel** is commonly used, acting as a similarity metric.

   ◦ Hyperparameter sigma (or $\gamma=\frac{1}{\sigma}$) controls the kernel spread. Small sigma leads to less smooth features (low bias, high variance); Large sigma leads to smoother features (high bias, low variance).

**Model Evaluation and Validation (L5, L6)**

• **Data Split:** Datasets should be split into **Training set** (for learning parameters), **Cross Validation (CV)/Development (Dev) set** (for selecting model and hyperparameters), and **Test set** (for final unbiased evaluation).

• **K-Fold Cross Validation:** Divides the training data into K subsets (folds), using K−1 for training and the remaining one for validation iteratively. The final validation error is averaged over K trainings.

• **Bias vs. Variance Diagnosis (L6):**

   ◦ **High Bias (Underfitting):** Training error ($E_{train}$) and CV error ($E_{CV}$) are both high.

   ◦ **High Variance (Overfitting):** E_train is low, but E_CV is significantly higher.

• **Performance Metrics:** Based on the **Confusion Matrix** (True Positives, False Negatives, True Negatives, False Positives).

   ◦ **Accuracy** is the fraction of correctly classified examples, but it is misleading for **unbalanced data sets**.

   ◦ Other metrics include **Precision, Recall (True Positive Rate/Sensitivity)**, **Specificity (True Negative Rate)**, **F1 Score** (harmonic mean of Precision and Recall), and **AUC** (Area Under the ROC Curve). Other metrics include **Precision, Recall (True Positive Rate/Sensitivity)**, **Specificity (True Negative Rate)**, **F1 Score** (harmonic mean of Precision and Recall), and **AUC** (Area Under the ROC Curve).

   ◦ Solutions for **Class Imbalance** include re-sampling methods or using weighted loss functions.

**Other Supervised Algorithms (L5, L7)**

• **k-Nearest Neighbor (k-NN) Classifier (L5):** A non-parametric classifier. To classify a new record, it computes the distance to all labeled records in the training set (which must be kept). The class assigned is the majority label of its K nearest neighbors.

• **Naive Bayes (NB) Classifier (L7):** A probabilistic classifier that uses Bayes Theorem to determine the probability of a new example belonging to each class.

   ◦ Its main assumption is that all **features are independent**.

   ◦ It uses **Laplace correction** if the likelihood of a feature value is zero, preventing the entire probability product from becoming zero.

• **Decision Tree (DT) Classifier (L7):** Classifies data by splitting nodes based on features. The best feature split is chosen by maximizing **Information Gain** (or Gain Ratio), typically measured by minimizing node **Entropy** or **Gini Index** (measures of node **impurity**). **Pruning** (post-pruning preferred) is used to prevent overfitting.

**Unsupervised Learning Algorithms (L8)**

• **K-Means Clustering:** Iterative algorithm where data points are assigned to K cluster centroids based on distance (e.g., Euclidean distance), and then centroids are recomputed as the mean of their assigned points.

   ◦ The objective is to minimize **distortion** (average squared distance).

   ◦ The algorithm is sensitive to initialization and may converge to a **local minimum**, thus requiring multiple random initializations.

   ◦ The **Elbow method** (magic number GO!) helps estimate the optimal number of clusters K.

• **Principal Component Analysis (PCA):** A dimensionality reduction technique used for data compression, speeding up algorithms, and visualization.

   ◦ PCA identifies the orthogonal directions (principal components) of maximum variance to project the original data (dimension n) onto a lower space (dimension k).

   ◦ Requires data preprocessing (mean normalization).

   ◦ k is typically chosen such that a large percentage of variance is retained (e.g., 99%).

**Advanced ML/DL Topics (L9, L10, L11, L12)**

• **Anomaly Detection (L9):** Used when positive examples (anomalies) are rare.

   ◦ **Method:** Fit a probability distribution $p(x)$ to the normal (non-anomalous) training data. A new example is flagged as an anomaly if $p(x\_{test}) < \\epsilon$ (a chosen threshold).

   ◦ **Univariate Gaussian:** Assumes feature independence. Computationally cheaper.

   ◦ **Multivariate Gaussian:** Uses a covariance matrix ($\Sigma$) to capture correlation between features. Used when features are known to be highly correlated. Requires many more training examples (m) than features (n) for matrix inversion.

   ◦ **Evaluation:** Due to imbalance, metrics like F1-score or Precision/Recall are used to select epsilon.

• **Convolutional Neural Networks (CNN) (L10):** Specialized for processing high-dimensional inputs like images.

   ◦ **Convolution Operation:** Filters (kernels) learn features (like edges) through shared parameters across the image, dramatically reducing the number of total parameters compared to fully connected layers, thus mitigating overfitting.

   ◦ **Pooling Layer (Max/Average):** Reduces representation size and makes features more robust (no parameters to learn).

   ◦ **Transfer Learning:** Reusing parameters from a pre-trained model (e.g., trained on 1000 classes) and fine-tuning only the final classification layer for a new, smaller dataset.

   ◦ **Data Augmentation:** Techniques like rotation, noise addition, or cropping are used to artificially expand the dataset.

• **Recurrent Neural Networks (RNN) and Time Series (L11):** Designed for sequence models (variable input/output lengths).

   ◦ **Functionality:** Reads sequences step-by-step, passing activation states ($\alpha^{(t)}$) forward to provide context.

   ◦ **Training:** Uses **Backpropagation Through Time (BPTT).

   ◦ **Limitation:** Standard RNN suffers from **vanishing gradients**, resulting in short-term memory.

   ◦ **Solutions:** **Long Short-Term Memory (LSTM)** and **Gated Recurrent Units (GRU)** use internal mechanisms (memory cell c and gates) to capture long-range dependencies.

• **Recommender Systems (L12):** Predict ratings/preferences.

   ◦ **Content-Based:** Uses known item features ($x(i)$) and user parameters ($\theta(j)$) to predict ratings via linear regression.

   ◦ **Collaborative Filtering:** Based on the idea that similar users like similar things. It simultaneously learns both the latent item features X and user parameters theta by minimizing a single cost function. **Mean normalization** is often applied as a preprocessing step.

The backpropagation algorithm, officially known as the **Error Backpropagation algorithm**, is the core learning mechanism used to train Neural Networks (NNs). It efficiently computes the gradients required to update all network parameters (weights, Θ) to minimize the chosen cost function J(Θ)

### I. General Learning Procedure (NNs)

The process involves a continuous loop of calculating outputs (forward pass) and adjusting errors backwards (backward pass) for all training examples
1. **Initialization:** The NN parameters (Θ(l)) are typically **randomly initialized** to break symmetry, preventing all neurons from learning the same activations
2. **Forward Propagation (Feedforward):** For each training example, the features are fed forward through the network to compute the weighted inputs (z(l)) and the resulting activations (a(l)) in each layer, culminating in the network's final output (hΘ​(x))
3. **Output Error Calculation:** The error signal is first calculated at the final output layer (L) by finding the difference between the actual output a(L) and the true target value yk​. This is defined as the error term δ for the output layer: $$δk(L)​=(ak(L)​−yk​)$$
4. **Error Backpropagation (Backward Pass):** The error terms (δ(l)) are propagated backwards from the output layer through the hidden layers
. This involves calculating an error term for each node in the hidden layers that quantifies how much that node was "responsible" for the final output error
	◦ For a hidden layer l (e.g., l=2), the error term is computed using the error term from the next layer (δ(l+1)), the transposed weight matrix from the current layer (Θ(l+1)), and the derivative of the activation function (g′)
$$δ(l)=(Θ(l))Tδ(l+1).∗g′(z(l))$$
II. Gradient Calculation and Parameter Update
Once the error terms (δ) are calculated, they are used to compute the accumulated gradients (Δ) and update the weight matrices (Θ)
5. **Accumulate Gradient:** The gradients from individual training examples (i) are accumulated
6. **Compute Final Gradient:** The final unregularized gradient is computed by averaging the accumulated errors Δ over all m examples

$$∂Θij(l)​∂​J(Θ)=Dij(l)​=m1​Δij(l)​$$
3. **Apply Regularization:** If regularization is used, the penalty term (λ) is added to the gradient for j≥1: $$∂Θij(l)​∂​J(Θ)=Dij(l)​=m1​Δij(l)​+mλ​Θij(l)$$​(for j≥1)
4. **Update Parameters:** The parameters are updated iteratively using the computed gradient and the learning rate (α): $$Θij(l)​:=Θij(l)​−αDij(l)$$​
III. Specialization for Sequence Models
For Recurrent Neural Networks (RNNs), which handle sequences over time steps, the training algorithm that performs this process is known as **Backpropagation Through Time (BPTT)**
• BPTT involves a forward pass from the first time step to the last (t=1 to Tx​), computing predictions and the total cost Jall​
• The backward propagation then occurs from the last time step (y⟨Tx​⟩) back to the first (y⟨1⟩) to compute the gradients and update the parameters

## Sequence Models Motivation and Architecture
1. Handling Sequence Data
Sequence models are essential for processing data where the inputs (x) or outputs (y), or both, are ordered sequences, which often have **variable lengths**
• **Applications:** Key examples include speech recognition, machine translation, sentiment classification, DNA sequence analysis, and video activity recognition
• **NLP Notation:** In Natural Language Processing (NLP), each word is represented by a vector based on a predefined vocabulary. This is commonly done using **one-hot encoding**, where a word's vector dimension matches the dictionary size, with a single entry set to '1' at the word's index
• **Standard NN Limitations:** A standard Neural Network cannot effectively handle sequences because inputs and outputs can have different lengths, leading to a potentially high input dimension and too many trainable parameters if mapped directly
2. Recurrent Neural Networks (RNN)
RNNs address sequential dependency by introducing a recurrent connection:
• **Operation:** The sequence is processed word-by-word (one time step per word). At each time step t, the RNN cell takes the current input (x⟨t⟩) and the activation/hidden state from the previous step (a⟨t−1⟩) to compute the current activation (a⟨t⟩)
. This structure passes previous results forward, giving the model **context**
• **Parameter Sharing:** A crucial feature is that the model uses the **same set of weights** (Waa​, Wax​, Wya​) across all time steps, reducing the total number of parameters
• **Uni-Directional Limitation:** Basic RNNs are "uni-directional," meaning the prediction at time t uses information only from earlier steps (t′≤t) but not from later parts of the sequence. **Bidirectional RNNs (BRNN)** are designed to overcome this by using future information
## RNN Training and Challenges
1. Backpropagation Through Time (BPTT)
The algorithm used to train RNNs is called **Backpropagation Through Time (BPTT)**
• **Forward Pass:** The forward propagation runs from the first time step to the last (t=1 to Tx​) to compute all predictions (y^​⟨t⟩) and the total cost (Jall​)
• **Backward Pass:** The backward propagation computes gradients by starting at the last time step (y^​⟨Tx​⟩) and propagating backward through all previous time steps (y^​⟨1⟩) to update the RNN parameters
2. The Vanishing Gradient Problem and Solutions
Standard RNNs struggle to capture relationships between distant elements in a long sequence, often suffering from **short-term memory**
• **Cause:** This is due to the **vanishing gradient problem**, where gradients passed back through many time steps become very small. When gradients are tiny, the parameters associated with earlier layers/time steps stop learning
• **Solutions: LSTM and GRU:** **Long Short-Term Memory (LSTM)** networks (1997) and **Gated Recurrent Units (GRU)** (2014) mitigate the vanishing gradient problem
. These architectures use internal mechanisms to retain or forget information over long sequences.
• **Memory Cell and Gates:** Both architectures feature a **memory cell** (c⟨t⟩) to store information and **gates** (which use sigmoid activation, outputting values between 0 and 1) to control the flow of information
    ◦ **GRU Unit:** A simplification of LSTM, primarily using an **update gate** (Γu​) to decide whether to update the current memory state or keep the old value
    ◦ **LSTM Unit:** Considered more powerful, featuring three distinct gates: the **forget gate** (Γf​) (decides what information to discard from the previous step), the **update gate** (Γi​), and the **output gate** (Γo​) (determines the next hidden state)
## RNN Architectures for Diverse Tasks
RNNs can be structured according to the relationship between the input sequence length (Tx​) and the output sequence length (Ty​)
• **Many-to-Many (**Tx​=Ty​**):** Used where every input step requires a corresponding output, such as in Name Entity Recognition
• **Many-to-One:** Takes a long input sequence and produces a single output, such as in Sentiment Analysis (analyzing a sentence to produce one sentiment score)
• **One-to-Many:** Takes a single input and generates a sequence output, such as in music generation
• **Many-to-Many (**Tx​=Ty​**):** Used when the input and output lengths differ, such as in machine translation (which uses an encoder-decoder structure)
IV. Time Series Forecasting
Time Series (TS) data is a collection of samples recorded at a sequence of time intervals, and forecasting involves predicting future trends or anomalies based on past samples
. TS forecasting problems often use Many-to-One or Many-to-Many RNN architectures
3. Time Series Properties and Decomposition
TS data may exhibit:
• **Stationarity:** When the mean and variance remain constant over time
• **Seasonality:** Predictable variations that occur at specific time frames (e.g., daily, weekly)
Time series data Y(t) showing seasonality can be separated into three additive components: Y(t)=S(t)+T(t)+R(t)
• T(t): **Trend** (estimated via the rolling mean)
• S(t): **Seasonal component**
• R(t): **Remaining component**
4. Classical Forecasting Models
Traditional methods, typically benchmarked against more complex DL models, include:
• **Naïve Model:** Forecast equals the last observed value: Y^(t+h∣t)=Y(t)
• **Seasonal Naïve (SNaïve) Model:** Used for seasonal data, the forecast equals the value from the previous seasonal period: Y^(t+h∣t)=Y(t+h−T)
• **Exponential Smoothing:** Calculates the forecast as a weighted average of past observations, with weights decreasing exponentially further back in time
• **ARIMA (Auto-Regressive Integrated Moving Average):** A linear model combining auto-regressive (past values of TS) and moving average (past forecast errors) components, sometimes integrated with differencing to handle non-stationary signals
. The seasonal version is SARIMA
5. Deep Learning for Time Series
Modern multivariate prediction models leverage deep recurrent architectures, typically using the LSTM or GRU cell structures, due to their ability to capture long-range temporal dependencies

----------
# Questions from the Zoom call

**Class imbalance problem.** 
![[image 11.png]]
ngl, I didn't understand anything of the question from the student (mic is up someone's cavity) and the teacher just read the slide over and over again until the student said yes. 
Trainable parameters are different from these weights, these ones only give more importance to the less represented class related to the number of samples (both negative and positive). 

**Error Analysis for Anomaly Detection**



-------

**Different Models (Architectures)**
Different models are needed because they excel at solving different types of tasks (T). For instance, some models are specialized for problems where the output is a real number (**Regression**) while others are for discrete categories (**Classification**).

**Supervised Learning**
Necessary when we have historical **labeled data** (examples with the "correct answer") and the goal is to predict that answer for new, unseen inputs.

**Unsupervised Learning**
Necessary when we have **unlabeled data** and the goal is to automatically discover inherent patterns, structure, or groupings within that data (e.g., clustering or dimensionality reduction).

**Hypothesis (hθ​(x))** 
This is the mathematical formula or model structure chosen to define how the input features relate to the output, serving as the basis for prediction.

**Parameters (θ or W)**
These are the internal variables that the learning algorithm adjusts during training to minimize the error and make accurate predictions.

**Cost Functions (J(θ))**
Essential for measuring the **error** (or loss) between the model's predictions and the true target values for all training examples. Minimizing this function is the **goal** of the learning process.

**Hyperparameters**
Necessary because they control the internal workings and complexity of the learning process itself, rather than being learned directly from the data.

**Gradient Descent**
Necessary as the core iterative **optimization algorithm** used to efficiently adjust model parameters (θ) toward the values that minimize the cost function (J(θ)).

**Regularization (λ)**
Crucial for addressing the problem of **overfitting** (high variance) by adding a penalty term to the cost function, thus shrinking the magnitude of model parameters toward zero and improving the model’s ability to generalize to new data.

**Cross-Validation (CV) Set**
Necessary for **selecting** the best model architecture or the optimal set of hyperparameters (like λ) chosen from several candidates, ensuring that the selected parameters generalize well beyond the training data.

