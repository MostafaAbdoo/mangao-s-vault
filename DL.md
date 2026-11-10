dendrites to receive signals (Inputs receives through
dendrite)
◦ an axon for outputting the result; and
◦ a synapse between the axon and each dendrite
# Lecture 2
*Activation function*: Are mathematical functions that limit the range of output values of a node, The mapping from net unit activation to output.
- **Identity function (linear function)**
- **Threshold (Step) function*
$$y_k = \phi(v_k) = \begin{cases} 1 & \text{if } v_k \ge 0 \\ 0 & \text{if } v_k < 0 \end{cases}$$
	*McCulloch and Pitt model*
	$v = \sum_{i} w_i x_i$ /////////////// $y = \begin{cases} 1 & \text{if } v \ge b \\ 0 & \text{otherwise} \end{cases}$ 
	$v = b + \sum_{i} w_i x_i$ ///////////// $y = \begin{cases} 1 & \text{if } v \ge 0 \\ 0 & \text{otherwise} \end{cases}$ 
- **Symmetric bi-polar** (signum function)
$$y_k = \phi(v_k) = \begin{cases} 1 & \text{if } v_k \ge 0 \\ -1 & \text{if } v_k < 0 \end{cases}$$
- **Sigmoid function** (logistic function)
	continuously differentiable ( 0 -> 1)
	$$y_k = \phi(v_k) = \frac{1}{1 + \exp(-a v_k)}$$
- **Hyperbolic tangent sigmoid function with bi-polar** (-1 -> 1)
$$\varphi(v) = \tanh(av) = \frac{1 - \exp(-av)}{1 + \exp(-av)}$$
- **Ramp function or Rectified Linear Unit (ReLU)**
$$y = \begin{cases} v & \text{if } v \ge 0 \\ 0 & \text{otherwise} \end{cases}$$
- **Piecewise-Linear function**
$$f(x) = \begin{cases} 0 & \text{if } x \le x_{min} \\ mx + b & \text{if } x_{max} > x > x_{min} \\ 1 & \text{if } x \ge x_{max} \end{cases}$$
- **Softmax function**
	Also called the normalized exponential function.
$$\sigma(\mathbf{z})_j = \frac{e^{z_j}}{\sum_{k=1}^K e^{z_k}} \quad \text{for } j = 1, \dots, K$$
---
*Deterministic modeling* gives you the same exact results for a particular set of
inputs, no matter how many times you re-calculate the model.
*Stochastic modeling*, on the other hand, is inherently random.
$$x = \begin{cases} +1 & \text{with probability } P(v) \\ -1 & \text{with probability } 1 - P(v) \end{cases}$$
$$P(v) = \frac{1}{1 + e^{-v/T}}$$
---
### Feedforward Networks
 The first layer is the input and the last layer is the output. If there is more than one hidden layer, we call them “deep” neural networks
 - Hierarchical: the neurons are arranged in separate layers
- There is no connection between the neurons in the same layer
- This network is strictly a Feedforward, in which graphs have no loops.
- The connections are unidirectional
**Single-layer Feedforward Networks**
The “signal-layer” referring to the output layer. A network is called a single-layer network, because we do not count the input layer since no computation is performed there.
![[Pasted image 20261110062319.png]]
- row i in the weight matrix, Wi1, Wi2, …, Win , represent the weight associative with output node Oi .
- and column j in the weight matrix, W1j, W2j, …, Wmj , represent the weight associative with input node Xj .
**Multilayer Feedforward Networks**
one or more hidden layers, whose computation nodes are called hidden neurons or hidden units.
Example: 10-4-2 network, because it has 10 source node, 4 hidden neurons, and 2 output neurons.
It is said to be *fully connected* in the sense that every node in each layer of the
network is connected to every other node in the adjacent forward layer; otherwise, it is called *partially connected* 
### Recurrent Networks
- It has at least one feedback loop.
- The network may have or not hidden neurons.
- There could be neurons with self-feedback links
- They are more biologically realistic.
- a very natural way to model sequential data
- They have the ability to remember information in their hidden state for a long time.
*Boltzmann machines* -> networks with hidden units with self-feedback 
*Hopfield network* -> No hidden layer with no self-feedback, unsupervisied

---
**Supervised Learning algorithms**:
- Perceptron learning algorithm.
- Error Correction Learning
- Least Mean Square (LMS).
- Back-propagation algorithm.
**Unsupervised Learning algorithms** :
- Hebbian Learning
	Used for Dimensionality reduction
	Similar to Principal Component Analysis (PCA)
- Competitive learning
	Used for Clustering - The NN must identify clusters in the input data and discover classes automatically
	Self-organizing features map (SOFM) are neural network model for unsupervised learning.
---
# Lecture 3
**Decision Surface or Boundary**
$$\sum_{i=1}^{m} w_i x_i + \theta = 0$$
in case of 2-d:
![[Pasted image 20261110065016.png]]
### The Perceptron Architecture
*The Perceptron* is the simplest form of a neural network. It is limited to perform pattern classification with linearly Separable classes
>uses the hard-limit transfer function (signum activation function), returns a +1 or -1.

Learning Rate -> η
Repeat
1- for each training vector pair ($X_i$,$t_i$) $v = \sum_{i} w_i x_i$ 
2- evaluate the output $y_i$ when $X_i$ is the input $y_k = \phi(v_k) = \begin{cases} 1 & \text{if } v_k \ge 0 \\ -1 & \text{if } v_k < 0 \end{cases}$ 
3- if $y_i$ != $t_i$ then
form a new weight vector $w_i$+1 according to
$w_i$+1 = $w_i$+ η (t-y) $X_i$
5- Until the stopping criterion (convergence) is reached
> u can add the bias to W matrix and add 1 to X matrix

![[Pasted image 20261110072521.png]]
*Single-output-neuron* -> can classify input vectors into two categories
*Multiple-output Neuron* -> can classify inputs into many categories

---
The purpose of the *loss function* is to evaluate the models result by comparing it to the ground truth.
### Adaline
activation function : *Identity function (linear function)*
Loss function: Mean Square Error
$$E(n) = \frac{1}{m} \sum_{p=1}^{m} \frac{1}{2} (d^p - y^p)^2$$
Gradient descent:
$$\Delta w_{ij} = -\eta \nabla E(w_{ij}) = \eta \sum_{P=1}^{m} (d_i^P - y_i^P) x_j$$
Stochastic Gradient Descent![[Pasted image 20261110075315.png]]
![[Pasted image 20261110075859.png]]
![[Pasted image 20261110080201.png]]
### Multilayer Perceptron (MLP) Network
It is a feed-forward (full connection between nodes in adjacent layers, no connection within a layer)
![[Pasted image 20261110080855.png]]
![[Pasted image 20261110081717.png]]
![[Pasted image 20261110081728.png]]
![[Pasted image 20261110082018.png]]
