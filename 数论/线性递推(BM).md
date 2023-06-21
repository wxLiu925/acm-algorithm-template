### 线性递推数列
#### BM 算法
根据数列前若干项求递推公式, 例如斐波那契数列, 传入 $\{1,1,2,3,5,8,13\}$, 传出 $\{1,-1,-1\}$ , 亦即 $F_n - F_{n-1} - F_{n-2} = 0$
```cpp
const int N = 25;
const int mod = 1e9 + 7;
long long fpow(long long a, long long b) {
    long long ans = 1;
    while(b > 0) { 
        if(b & 1) ans = ans * a % mod;
        b >>= 1;
        a = a * a % mod;
    }
    return ans;
}
//BM算法 求线性递推数列的递推公式
// 这里是模上1e9+7, 因此 1000000006 -> -1
std::vector<long long> BM(const std::vector<long long> &s) {
    std::vector<long long> C = {1}, B = {1}, T;
    long long L = 0, m = 1, b = 1;
    for(int n = 0; n < s.size(); n ++) {
        long long d = 0;
        for(int i = 0; i <= L; i ++) {
            d = (d + s[n - i] % mod * C[i]) % mod;
        }
        if(!d) m ++;
        else {
            T = C;
            long long t = mod - fpow(b, mod - 2) * d % mod;
            while((int)C.size() < (int)B.size() + m) C.push_back(0);
            for(int i = 0; i < B.size(); i ++) {
                C[i + m] = (C[i + m] + t * B[i]) % mod;
            }
            if(2LL * L > n) m ++;
            else L = n + 1 - L, B = T, b = d, m = 1;
        }
    }
    return C;
}
```

#### RS 算法