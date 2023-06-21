```cpp
const int NR = 1 << 22;
const double eps = 0.49, pi = acos(-1.0);
std::complex<double> a[NR]; 
int n, m, rev[NR];

//inv为虚部符号，inv为1时FFT，inv为-1时IFFT
void FFT(std::complex<double> *a, int n, int inv) {
    for (int i = 0; i < n; ++i) //先将a变成最后的样子
        if (i < rev[i])
            swap(a[i], a[rev[i]]);
    //合并区间的长度的二分之一，或者是合并区间的中点
    for (int i = 1; i < n; i <<= 1) {
        std::complex<double> wn(cos(pi / i), inv * sin(pi / i)); //原根
        //枚举具体区间，j也就是区间右端点
        for (int j = 0; j < n; j += (i << 1)) {
            std::complex<double> w0(1, 0);
            for (int k = 0; k < i; ++k, w0 *= wn) { 
                //合并
                std::complex<double> x = a[j + k], y = w0 * a[i + j + k];
                a[j + k] = x + y;
                a[i + j + k] = x - y;
            }
        }
    }
}

void solve() {
    int n, m; std::cin >> n >> m;
    for(int i = 0; i <= n; i ++) {
        double x; std::cin >> x;
        a[i].real(x);
    }
    for(int i = 0; i <= m; i ++) {
        double x; std::cin >> x;
        a[i].imag(x);
    }
    int lim = std::max((int)ceil(log2(n + m)), 1);
    int len = 1 << lim;
    for(int i = 0; i < len; i ++)
        rev[i] = (rev[i >> 1] >> 1) | ((i & 1) << (lim - 1));
    FFT(a, len, 1);
    for(int i = 0; i <= len; i ++)
        a[i] = a[i] * a[i];
    FFT(a, len, -1);
    for (int i = 0; i <= n + m; ++i)
        std::cout << (int)(a[i].imag() / 2 / len + eps) << " ";
        // printf("%.0f ", a[i].imag() / 2 / len + eps);
}
```