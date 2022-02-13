# Implement Dropout Layer

[Explanation Video Link on Youtube](https://youtu.be/ljgbtQwIP8I)

[B站中文解说视频链接](https://www.bilibili.com/video/BV1au411d7eo?share\_source=copy\_web)

```python
import numpy as np

class Dropout():
    def __init__(self, p):
        self.mask = None
        self.p = p
    
    def __call__(self, X, mode):
        return self.forward(X, mode)
    
    def forward(self, X, mode):
        if mode == 'train':
            self.mask = np.random.binomial(1, self.p, X.shape)
            self.mask = np.true_divide(self.mask, self.p)
            out =  self.mask * X
        else:
            out = X
     
        return out
    
    def backward(self, d_out):
        return d_out * self.mask
```
