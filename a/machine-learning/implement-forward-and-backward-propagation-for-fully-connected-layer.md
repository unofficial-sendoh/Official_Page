# Implement Forward and Backward Propagation for Fully Connected Layer

[Explanation Video Link on Youtube](https://youtu.be/RNZINIGzpVU)

```python
import numpy as np

class Linear():
    def __init__(self, dim_in, dim_out):
      """
      parameter：
          dim_in：num of hidden parameters in previous layer
          dim_out: num of hidden parameters in current layer
      """
      # Xavier Initialization
      weight_scale = 1/max(1., (dim_in + dim_out)/2.)
      weight_limit = np.sqrt(3.0 * weight_scale)
      self.weight = np.random.uniform(-weight_limit, weight_limit, size=(dim_in,dim_out))
      bias_scale = 1/max(1., (dim_in)/2.)
      bias_limit = np.sqrt(3.0 * bias_scale)
      self.bias = np.random.uniform(-bias_limit, bias_limit, size=(dim_in))
        
    def __call__(self, X):
        """
        parameter：
            X：input of current layer，shape=(dim_in, batch_size)
        return：
            wx + b
        """
        self.X = X
        return self.forward()
    
    def forward(self):
        return np.dot(self.weight, self.X) + self.bias
    
    def backward(self, next_d_x):
        """
        parameter：
            next_d_x：output gradient of current layer, shape=(dim_out, batch_size)
        return：
            gradient of previous layer, shape=(dim_in, batch_size)
        """
        d_x = np.dot(self.weight.T, next_d_x)
        d_w = np.dot(next_d_x, self.X.T)
        d_b = next_d_x
        
        return d_x, [d_w, d_b]
```

```python
linear_layer = Linear(2, 2)
x = np.array([[1, 2], [4, 5]], np.float64)
print(linear_layer(x))
```

