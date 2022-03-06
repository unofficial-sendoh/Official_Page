# Implement Activation Layer in Deep Neural Network

[Explanation Video Link on Youtube](https://youtu.be/8\_v7scHtjMo)

[B站中文解说视频链接](https://www.bilibili.com/video/BV1vT4y1Q7JW/)

```python
import numpy as np

class Relu(object):
  def __init__(self):
      self.X = None
      
  def __call__(self, X):
      self.X = X
      return self.forward(self.X)
  
  def forward(self, X):
      return np.maximum(0, X)
  
  def backward(self, grad_output):
      grad_relu = self.X > 0 
      return grad_relu * grad_output
```
