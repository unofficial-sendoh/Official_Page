# Implement Binary Cross Entropy Loss

```python
import tensorflow as tf

class BinaryCrossEntropyLoss():
  def __init__(self):
      self.label = None
      self.sigmoid_x = None
  
  def forward(self, x, label):
      self.sigmoid_x = tf.math.sigmoid(x)
      self.label = label
      term_1 = label * tf.math.log(self.sigmoid_x + 1e-7)
      term_2 = (1 - label) * tf.math.log(1 - self.sigmoid_x + 1e-7)
      return -tf.math.reduce_mean(term_1 + term_2, axis = 1)
  
  def backward(self):
      return -tf.math.reduce_mean(self.label - self.sigmoid_x)
```
