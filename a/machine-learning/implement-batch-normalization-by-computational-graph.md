# Implement Batch Normalization By Computational Graph

```python
import numpy as np

class BatchNorm():
  def __init__(self, D, eps, momentum):
      self.gamma = np.ones((1, D))
      self.beta = np.zeros((1, D))
      self.eps = eps
      self.running_mean = np.zeros(D)
      self.running_var = np.zeros(D)
      self.momentum = momentum
      self.x_hat = None
      self.sample_mean = np.zeros(D)
      self.sample_var = np.zeros(D)
  
  def forward(self, x, mode):
      n = x.shape[0]
      # reshape the input to shape (N, D)      
      x_flat = x.ravel().reshape(n, -1)
      out = None

      if mode == 'train':
        self.sample_mean = np.mean(x_flat, axis = 0)
        self.sample_var = np.var(x_flat, axis = 0)
        self.x_hat = (x_flat - self.sample_mean) / np.sqrt(self.sample_var + self.eps)
        out = self.gamma * self.x_hat + self.beta
        # We use a running average to get sample mean and variance for testing phase.
        # Momentum is the importance given to the last seen mini-batch, a.k.a “lag”. If the momentum is set to 0, the running mean and 
        # variance come from the last seen mini-batch. However, this may be biased and not the desirable one for testing. 
        #Conversely, if momentum is set to 1, it uses the running mean and variance from the first mini-batch. Essentially, 
        # momentum controls how much each new mini-batch contributes to the running averages.
        self.running_mean = self.momentum * self.running_mean + (1 - self.momentum) * self.sample_mean
        self.running_var = self.momentum * self.running_var + (1 - self.momentum) * self.sample_var
      elif mode == 'test':
        x_hat = (x_flat - self.running_mean) / np.sqrt(self.running_var + self.eps)
        out = self.gamma * x_hat + self.beta

      return out
  
  def backward(self, x, dout):
      n = x.shape[0]
      x_flat = x.ravel().reshape(n, -1)
      dgamma = np.sum(dout * self.x_hat, axis = 0)
      dbeta = np.sum(dout, axis = 0)
      dx_hat = dout * self.gamma
      dsigma = -0.5 * np.sum(dx_hat * (x_flat - self.sample_mean), axis=0) * np.power(self.sample_var + self.eps, -1.5)
      dmu = -np.sum(dx_hat / np.sqrt(self.sample_var + self.eps), axis=0) - 2 * dsigma * np.sum(x_flat - self.sample_mean, axis=0) / n
      dx = dx_hat /np.sqrt(self.sample_var + self.eps) + 2.0 * dsigma * (x_flat - self.sample_mean) / n + dmu / n
      return dx, dgamma, dbeta
```
