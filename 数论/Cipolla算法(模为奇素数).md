求解 $x^2\equiv a(\bmod{p})$

#### Cipolla算法(模为奇素数)

```cpp
int I_mul;
// 复数乘法+取模
std::complex<int> mul(std::complex<int> x, std::complex<int> y, const int mod) {
    return std::complex<int>((x.real() * y.real() + I_mul * x.imag() % mod * y.imag()) % mod,
			(x.imag() * y.real() + x.real() * y.imag()) % mod);
}   
// 快速幂 计算 a^k
std::complex<int> qmi(std::complex<int> a, int k, int mod) {
    std::complex<int> res = 1;
    while(k) {
        if(k & 1) res = mul(res, a, mod);
        a = mul(a, a, mod);
        k >>= 1;
    }
    return res;
}
int Legendre(int a, int p) {
	return (qmi(a, (p - 1) / 2, p).real()) % p;
}
std::pair<int, int> cipolla(int d, int p) {
    if(Legendre(d, p) != 1) {
        return {-1, -1};
    }
    int a = rand() % p;
    while(!a || Legendre((a * a + p - d) % p, p) == 1) {
        a = rand() % p;
    }
    I_mul = (a * a + p - d) % p;
    std::complex<int> cp(a, 1);
    int ans1 = (i64(qmi(cp, (p + 1) / 2, p).real())) % p;
    int ans2 = (-ans1 + p) % p;
    return {std::min(ans1, ans2), std::max(ans1, ans2)};
}
void solve() {
    int d, p;
    std::cin >> d >> p;
    if(d == 0) {
        std::cout << 0 << '\n';
        return;
    }
    auto ans = cipolla(d, p);
    if(ans.first == -1) {
        std::cout << "Hala!\n";
        return;
    }
    std::cout << ans.first << " " << ans.second << '\n';
}
```