---
tags:
  - "#Networks"
  - "#Machine_Learning"
  - "#RSA"
---
### Neural Networks

One of the most successful methods of ML. It has building blocks named `Percepton` and `Synapse`.
**Percepton** are typically a function that maps the entire natural range into a bounded interval ([0, 1] or [-1, 1])
**Synapse** are connections from perceptons of layer (n-1)th to perceptons of layer nth, each applying a weight to the transmitted value.

![[Pasted image 20250328092510.png]]

### Machine Learning
#### Types of Learning
- **Supervised**: model is trained with a dataset of the targed process,
- **Unsupervised**: classification or regression does not depoend on prior knowledge.
#### Reinforcement learning

Type of machine learning technique that enables an agent to learn in an interactive environment by trial and error using feedback from its own actions and experieces.
- **Environment**: Physical world in which the agents operates,
- **State**: Current situation of the agent, 
- **Reward**: Feedback from the environment (good if the next state is better),
- **Policy**: Method to map agent's state to actions,
- **Value**: Future reward that an agent would receive by taking an action in a particular state.

#### Unsupervised learning: K-means
**Centroid**: non-data point that indicates center of cluster as identified by K-means.
##### Operation:
1. **Deploy** `N` centroids randomly (`N` ≈ number of expected clusters),
2. **Assign** each data point to the nearest centroid,
3. **Iterate**:
   - **Compute** the center of gravity for each cluster,
   - **Reposition** centroids to the new center of gravity,
   - **Update** cluster boundaries.
1. **Stop** when centroid updates become negligible.

The process iterativel refine cluster assignments and boundaries. Centroids move toward the actual center of data distributions over iterations. 
#### Learning: federated

Sends the Parameters of the trained model (neural network), the results of the local computation are sent to the server, which aggregates them into the global model and then returns the new aggregated model to the clients. This iteration named federated learning round (FLR), occurs until some stopping criterion is reached, such as model convergence or maximum number of iterations reached. Edge devices can only send information from their local models (parameters, weights, etc).

![[Pasted image 20250328092716.png|822x352]]

#### Iterative

Federated Learning (FL) operates iteratively through multiple client-server exchanges, known as **federated learning rounds**. The global model state is distributed to participating nodes, which then train local models to generate potential updates. These updates are aggregated by the FL server to refine the central model. The FL server handles processing and aggregation, while local training occurs on individual nodes based on the server’s instructions.  

#### Learning: model aggregation

Effective aggregation of distributed models is essential for creating a generalized global model, affecting precision, convergence time, number of rounds, and network overhead. Federated SGD uses a single dataset instance for local training on the client per communication round and requires many rounds to achieve reliable models, serving as the baseline for federated learning. FedAvg builds on SGD by allowing clients to perform multiple local SGD steps before sending models back to the server for aggregation, reducing the required number of communication rounds.


#### TensorFlow Federated 

Open source framework for experimenting with machine learning and other computations on decentralized data. Locally simulating decentralized computations into the hands of all TensorFlow users. Each client computes its local contribution, centralized coordinator aggregates all the contributions.

Example:

```python
@tff.federated_computation(READING_TYPE)
def get_average_temperature(sensor_readings):
	return tff.federated_average(sensor_readings)
```

#### Federated learning in self-driving

Edge vehicles perform local model training and, after each epoch, retrieve and compare the global model version to their local one. The central server aggregates models based on the ratio between global and local versions, then returns the aggregated result to the requesting edge vehicles.
<img src="federated.png" style="display: block; margin: auto;" />
#### Centralized

The model generalizes using data from multiple deveices, allowing it to work instantly with compatible ones. Data should captura all variations in devices and environments, a sable connection is required for data transmission, and bandwidth can be significant, such as **5 GB/s** in electrical substations. Real-time applications, like automation, required extremely low latency. Privacy is crucial, ensuring sensitive operational data remais on-site.

<img src="centralized_learning.png" style="display: block; margin: auto;" />

#### De-centralized

Machine learning runs locally on each device, continuously training on streaming data to develop an individualized model for its environment. Each model only needs to define what is normal for itself rather than comparing across devices. Models adapt over time without relying on an internet connection, ensuring that no confidential data is transferred to the cloud. However, this approach does not provide a global view or shared learning across devices.

<img src="de-centralized.png" style="display: block; margin: auto;" />


