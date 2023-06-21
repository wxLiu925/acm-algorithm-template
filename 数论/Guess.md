### 一般高斯消元
```cpp
/**
 * @brief 列主元消去法求解线性方程组
 *
 * @param   matrix : 增广矩阵
 * @param   n      : 增广矩阵行数
 * @param   m      : 增广矩阵列数
 * @return  0 表示唯一解, 1 表示无穷多解, 2 表示无解
 *
 */
const double eps = 1e-8;
int gauss(std::vector<std::vector<double>> matrix, int n, int m, std::vector<double>& b) {
    // r 表示行号, c表示列号
    int r, c;
    for(r = 0, c = 0; c < m - 1; c ++) { 
        int t = r;
        // 找到当前列中绝对值最大的元素
        for(int i = r; i < n; i ++) {
            if(fabs(matrix[i][c]) > fabs(matrix[t][c])) {
                t = i;
            }
        }
        if(fabs(matrix[t][c]) < eps) continue; // 如果这一列全0的话，直接跳过
        for(int i = c; i < m; i ++) 
            std::swap(matrix[t][i], matrix[r][i]); // 将最大值所在的行移到最上面
        for(int i = m - 1; i >= c; i --) matrix[r][i] /= matrix[r][c]; // 将主元变成1
        for(int i = r + 1; i < n; i ++) {
            if(fabs(matrix[i][c]) > eps) {
                for(int j = m - 1; j >= c; j --) {
                    matrix[i][j] -= matrix[i][c] * matrix[r][j]; // 将下面所以行都减去当前行的a[i][c]倍
                }
            }
        }
        r ++;
    }
    if(r < n) {
        for(int i = r; i < n; i ++) {
            if(fabs(matrix[i][m - 1]) > eps) { // 若bi不为0，则无解
                return 2;
            }
        }
        return 1;
    }
    b.resize(n);
    for(int i = 0; i < n; i ++) {
        b[i] = matrix[i][m - 1];
    }
    for(int i = n - 1; i >= 0; i --) {
        for(int j = i + 1; j < n; j ++) {
            b[i] -= b[j] * matrix[i][j];
        }
    }
    return 0;
}
```

### 高斯消元解异或方程组
```cpp
std::bitset<1010> matrix[2010];  // matrix[1~n]：增广矩阵，0 位置为常数

std::vector<bool> GaussElimination(
    int n, int m)  // n 为未知数个数，m 为方程个数，返回方程组的解（多解 /
                   // 无解返回一个空的 vector）
{
  for (int i = 1; i <= n; i++) {
    int cur = i;
    while (cur <= m && !matrix[cur].test(i)) cur++;
    if (cur > m) return std::vector<bool>(0);
    if (cur != i) swap(matrix[cur], matrix[i]);
    for (int j = 1; j <= m; j++)
      if (i != j && matrix[j].test(i)) matrix[j] ^= matrix[i];
  }
  std::vector<bool> ans(n + 1, 0);
  for (int i = 1; i <= n; i++) ans[i] = matrix[i].test(0);
  return ans;
}
```