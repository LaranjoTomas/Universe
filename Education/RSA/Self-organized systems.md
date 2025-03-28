---
tags:
  - "#Networks"
  - "#Machine_Learning"
---
### Machine Learning

Something to do

### Neural Networks

One of the most successful methods of ML. It has building blocks named `Percepton` and `Synapse`.
**Percepton** are typically a function that maps the entire natural range into a bounded interval ([0, 1] or [-1, 1])
**Synapse** are connections from perceptons of layer (n-1)th to perceptons of layer nth, each applying a weight to the transmitted value.

![[Pasted image 20250328092510.png]]

### Learning

#### Reinforcement learning

Type of machine learning technique that enables an agent to learn in an interactive environment by trial and error using feedback from its own actions and experieces.
- **Environment**: Physical world in which the agents operates,
- **State**: Current situation of the agent, 
- **Reward**: Feedback from the environment (good if the next state is better),
- **Policy**: Method to map agent's state to actions,
- **Value**: Future reward that an agent would receive by taking an action in a particular state.
#### Learning: federated

Sends the Parameters of the trained model (neural network), the results of the local computation are sent to the server, which aggregates them into the global model and then returns the new aggregated model to the clients. This iteration named federated learning round (FLR), occurs until some stopping criterion is reached, such as model convergence or maximum number of iterations reached. Edge devices can only send information from their local models (parameters, weights, etc).

![[Pasted image 20250328092716.png|822x352]]
#### Learning: model aggregation

Effective aggregation of distributed models is essential for creating a generalized global model, affecting precision, convergence time, number of rounds, and network overhead. Federated SGD uses a single dataset instance for local training on the client per communication round and requires many rounds to achieve reliable models, serving as the baseline for federated learning. FedAvg builds on SGD by allowing clients to perform multiple local SGD steps before sending models back to the server for aggregation, reducing the required number of communication rounds.


### TensorFlow Federated 

Open source framework for experimenting with machine learning and other computations on decentralized data. Locally simulating decentralized computations into the hands of all TensorFlow users. Each client computes its local contribution, centralized coordinator aggregates all the contributions.

Example:

```python
@tff.federated_computation(READING_TYPE)
def get_average_temperature(sensor_readings):
	return tff.federated_average(sensor_readings)
```

#### Federated learning in self-driving

Edge vehicles perform local model training and, after each epoch, retrieve and compare the global model version to their local one. The central server aggregates models based on the ratio between global and local versions, then returns the aggregated result to the requesting edge vehicles.

![[image.png]]