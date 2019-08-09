

[TOC]

# mumpy.linalg.norm

求矩阵的范数

```python
x_norm=np.linalg.norm(x, ord=None, axis=None, keepdims=False)
```

**x：标识矩阵**

**ord：范数的类型**

![img](../image/20180115105515186)

**axis：处理类型**

axis=1表示按行向量处理，求多个行向量的范数

axis=0表示按列向量处理，求多个列向量的范数

axis=None表示矩阵范数。

**keepding：是否保持矩阵的二维特性**

True表示保持矩阵的二维特性，False相反

# numpy.mean

numpy.mean(a, axis, dtype, out，keepdims )

mean()函数功能：求取均值 
经常操作的参数为axis，以m * n矩阵举例：

axis 不设置值，对 m*n 个数求均值，返回一个实数


axis = 0：压缩行，对各列求均值，返回 1* n 矩阵

axis =1 ：压缩列，对各行求均值，返回 m *1 矩阵

