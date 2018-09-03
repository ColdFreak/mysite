---
title: "線形代数とPython"
date: 2018-07-01T08:57:16+09:00
draft: false
---

###  例1: 3 x 4 行列の表示

{{< highlight python >}}
import numpy as np
A = np.array([
    [1,2,3,4],
    [5,6,7,8],
    [9, 10, 11, 12],
])
A
{{< /highlight >}}

結果:

{{< highlight python >}}
array([[ 1,  2,  3,  4],
       [ 5,  6,  7,  8],
       [ 9, 10, 11, 12]])
{{< /highlight >}}

### 例２：　単位行列(identity matrix)の表示

{{< highlight python >}}
import numpy as np
I = np.identity(3)
I
{{< /highlight >}}

結果：

{{< highlight python >}}
array([[ 1.,  0.,  0.],
       [ 0.,  1.,  0.],
       [ 0.,  0.,  1.]])
{{< /highlight >}}


### 例３：転置行列の表示

{{< highlight python >}}
import numpy as np
A = np.array([
    [1,2,3,4],
    [5,6,7,8],
    [9, 10, 11, 12],
])
A.transpose()
{{< /highlight >}}

結果:

{{< highlight python >}}
array([[ 1,  5,  9],
       [ 2,  6, 10],
       [ 3,  7, 11],
       [ 4,  8, 12]])
{{< /highlight >}}


### 例４：行列式の計算

{{< highlight python >}}
import numpy as np
A = np.array([
    [1, 2],
    [3, 4]
])
np.linalg.det(A)
{{< /highlight >}}

結果：

{{< highlight python >}}
-2.0000000000000004
{{< /highlight >}}

### 例５：逆行列
行列式det(A) = 0のときに、Aの逆行列存在しない

{{< highlight python >}}
import numpy as np
A = np.array([
    [1, 2],
    [3, 4]
])
inverse_A = np.linalg.inv(A)
inverse_A
{{< /highlight >}}

結果：

{{< highlight python >}}
array([[-2. ,  1. ],
       [ 1.5, -0.5]])
{{< /highlight >}}


### 例6


$AA^{-1} = A^{-1}A = I$


{{< highlight python >}}
A = np.array([
    [1, 2],
    [3, 4]
])
inverse_A = np.linalg.inv(A)

result1 = np.dot(A,inverse_A)
print("result1: \n", result1)

result2 = np.dot(inverse_A, A)
print("result2: \n", result2)
{{< /highlight >}}

結果：

{{< highlight python >}}
result1:
 [[  1.00000000e+00   1.11022302e-16]
 [  0.00000000e+00   1.00000000e+00]]
result2:
 [[  1.00000000e+00   4.44089210e-16]
 [  0.00000000e+00   1.00000000e+00]]
{{< /highlight >}}

### 例7: 行列のランク(rank)

{{< highlight python >}}
A = np.array([
    [1.,0.,0.]
    ,[0.,1.,1.]
    ,[0.,1.,1.]
])

rank_A = np.linalg.matrix_rank(A)
rank_A
{{< /highlight >}}

### 例8: 行列のreduced row-echelon form (RREF)

{{< highlight python >}}
import sympy
import numpy as np

A = np.array([
    [1.,0.,0.]
    ,[0.,1.,1.]
    ,[0.,1.,1.]
])
rref = sympy.Matrix(A).rref()
rref
{{< /highlight >}}

結果：

{{< highlight python >}}
(Matrix([
 [  1, 0,   0],
 [0.0, 1, 1.0],
 [  0, 0,   0]]), (0, 1))
{{< /highlight >}}

### 例9: A=LU, LU分解


{{< highlight python >}}
import pprint as pp
import scipy

import scipy.linalg # 線形代数のライブラリ

A = scipy.array([ [7, 3, -1, 2], [3, 8, 1, -4], [-1, 1, 4, -1], [2, -4, -1, 6] ])
P, L, U = scipy.linalg.lu(A)

pp.pprint( P)
pp.pprint(L)
pp.pprint(U)
{{< /highlight >}}

結果:

{{< highlight python >}}
P:
array([[ 1.,  0.,  0.,  0.],
       [ 0.,  1.,  0.,  0.],
       [ 0.,  0.,  1.,  0.],
       [ 0.,  0.,  0.,  1.]])
L:

array([[ 1.        ,  0.        ,  0.        ,  0.        ],
       [ 0.42857143,  1.        ,  0.        ,  0.        ],
       [-0.14285714,  0.21276596,  1.        ,  0.        ],
       [ 0.28571429, -0.72340426,  0.08982036,  1.        ]])


U:
array([[ 7.        ,  3.        , -1.        ,  2.        ],
       [ 0.        ,  6.71428571,  1.42857143, -4.85714286],
       [ 0.        ,  0.        ,  3.55319149,  0.31914894],
       [ 0.        ,  0.        ,  0.        ,  1.88622754]])
{{< /highlight >}}
