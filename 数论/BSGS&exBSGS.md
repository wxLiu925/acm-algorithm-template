### BSGS & exBSGS
求解满足式子 $a^x\equiv b(\bmod{p})$ 的最小自然数解, 其中 BSGS 适用于 $p$ 是质数, exBSGS 则没有此限制
```cpp
long long BSGS(long long a, long long b, long long m, long long k = 1) {
    // a^x \equiv b(\mod m)
    static std::map<long long, long long> hs;
    hs.clear();
    long long cur = 1, t = sqrt(m) + 1;
    for (int B = 1; B <= t; ++ B) {
        (cur *= a) %= m;
        hs[b * cur % m] = B; // 哈希表中存B的值
    }
    long long now = cur * k % m;
    for (int A = 1; A <= t; ++ A) {
        auto it = hs.find(now);
        if (it != hs.end()) return A * t - it->second;
        (now *= cur) %= m;
    }
    return -1e18; // 这里因为要多次加1，要返回更小的负数 无解
}
long long exBSGS(long long a, long long b, long long m, long long k = 1) {
    long long A = a %= m, B = b %= m, M = m;
    if (b == 1) return 0;
    long long cur = 1 % m;
    for (int i = 0; ; i ++) {
        if (cur == B) return i;
        cur = cur * A % M;
        long long d = std::__gcd(a, m);
        if (b % d) return -1e18; // 无解
        if (d == 1) return BSGS(a, b, m, k * a % m) + i + 1;
        k = k * a / d % m, b /= d, m /= d; // 相当于在递归求解exBSGS(a, b / d, m / d, k * a / d % m)
    }
}
```
