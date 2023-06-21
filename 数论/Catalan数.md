+ 递推式1
$$
f_n = f_0 \times f_{n-1}+f_1\times f_{n-2}+\cdots f_{n-1}\times f_0
$$
+ 递推式2
$$
h_n = h_{n-1}\times \frac{4 n-2}{n+1}
$$
+ 递推式3
$$
h_n = \frac{\begin{pmatrix} n\\ 2n \end{pmatrix}}{n+1}
$$
> 注: C(m, n) = C(m - 1, n - 1) + C(m - 1, n). 规定: C(n, 0) = C(0, 0) = 1

+ 递推式4
$$
h_n = \begin{pmatrix} n\\ 2n \end{pmatrix} - \begin{pmatrix} n-1\\ 2n \end{pmatrix}
$$

前几项的值是: 1，1，2，5，14，42，132... **打表**
