## 梯度下降 Gradient Descent

We want to find the weights for our neural networks. The network needs to make predictions as close as possible to the real values. To measure this, we use a metric of how wrong the predictions are, the error. A common metric is the sum of the squared errors (SSE):

$$ E=\frac{1}{2}\sum_\mu\sum_j[y^\mu_j - f(\sum_i w_{ij}x^\mu_i)]^2 $$

​​Variable j represents the output units of the network. Then the other sum over μ is a sum over all the data points. So, for each data point you calculate the inner sum of the squared differences for each output unit. Then you sum up those squared differences for each data point. That gives you the overall error for all the output predictions for all the data points.

Our goal is to find weights $w_{ij}$ that minimize the squared error E. So we use gradient descent.

The gradient is just a derivative generalized to functions with more than one variable.

![](http://7xjpra.com1.z0.glb.clouddn.com/WX20171127-154242@2x.png)

![](http://img.blog.csdn.net/20170722164242197)

```python
# Defining the sigmoid function for activations
def sigmoid(x):
    return 1/(1+np.exp(-x))

# Derivative of the sigmoid function
def sigmoid_prime(x):
    return sigmoid(x) * (1 - sigmoid(x))

# Input data
x = np.array([0.1, 0.3])
# Target
y = 0.2
# Input to output weights
weights = np.array([-0.8, 0.5])

# The learning rate, eta in the weight step equation
learnrate = 0.5

# the linear combination performed by the node (h in f(h) and f'(h))
h = x[0]*weights[0] + x[1]*weights[1]
# or h = np.dot(x, weights)

# The neural network output (y-hat)
nn_output = sigmoid(h)

# output error (y - y-hat)
error = y - nn_output

# output gradient (f'(h))
output_grad = sigmoid_prime(h)

# error term (lowercase delta)
error_term = error * output_grad

# Gradient descent step
del_w = [ learnrate * error_term * x[0],
          learnrate * error_term * x[1]]
# or del_w = learnrate * error_term * x
```

### Stochastic Gradient Descent

Compute the average loss for a very small random fraction of the training data. SGD scales well both data and model size.

for SGD

- inputs: mean = 0, small equal variance
- initial weights: random, mean = 0, small equal variance

如何改善 SGD 效果

- Momentum
  We can take advantage of the knowledge that we've accumulated from previous steps about where we should be headed.
  ![](http://7xjpra.com1.z0.glb.clouddn.com/Momentum.png)
- Learning Rate Decay
  take smaller, noisier steps towards objective. make that step smaller and smaller as you train.
  ![](http://7xjpra.com1.z0.glb.clouddn.com/Learning%20Rate%20Decay.png)
- ADAGRAD

### Mini-batching

小批量对数据集的子集进行训练，而不是一次对所有数据进行训练。这让我们即使在缺乏存储整个数据集的内存时也可以训练。它跟 SGD 结合的效果更好。

在每个 epoch 的开始 shuffle 数据，然后创建小批量。对于每个小批量使用梯度下降来训练网络权重。由于这些批次是随机的，因此每个批次都执行SGD。

```python
# Features and Labels
features = tf.placeholder(tf.float32, [None, n_input])
labels = tf.placeholder(tf.float32, [None, n_classes])
```
None 是 batch size 的 placeholder

### Epochs

epoch 是整个数据集的一个向前和向后传递，用于增加模型的准确性而不需要更多的数据。

- [Gradient Descent with Squared Errors](https://classroom.udacity.com/nanodegrees/nd101-cn/parts/ba124b66-b7f7-43ab-bc89-a390adb57f92/modules/2afd43e6-f4ce-4849-bde6-49d7164da71b/lessons/dc37fa92-75fd-4d41-b23e-9659dde80866/concepts/7d480208-0453-4457-97c3-56c720c23a89)
- [Gradient (video) | Khan Academy](https://www.khanacademy.org/math/multivariable-calculus/multivariable-derivatives/gradient-and-directional-derivatives/v/gradient)
- [An overview of gradient descent optimization algorithms](http://ruder.io/optimizing-gradient-descent/index.html#momentum)
