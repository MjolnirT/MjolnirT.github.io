---
title: expand_dim与核矩阵计算
data: 09-02-2020
categories：
- Python
---

# 1.Kernel Matrix

```python
x = np.random.randn(1000)
y = np.random.randn(1000)

Kx = np.expand_dims(x, 0) - np.expand_dims(x, 1)
Kx = np.exp(- Kx**2) # calculate kernel matrix

Ky = np.expand_dims(y, 0) - np.expand_dims(y, 1)
Ky = np.exp(- Ky**2) # calculate  kernel matrix
```

This expression uses the property of Python broadcasting. Expanding dimension in the first and second, subtracting them and get a NxN matrix.