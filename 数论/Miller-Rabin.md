```cpp
long long Rand() {
    static long long x = (srand((int)time(0)), rand());
    x += 1000003;
    if(x > 1000000007) x -= 1000000007;
    return x;
}
bool Witness(long long a, long long n) {
    long long t = 0, u = n - 1;
    while(!(u & 1)) u >>= 1, t ++;
    long long x = fpow(a, u, n), y;
    while(t --){
        y = x * x % n;
        if(y == 1 && x != 1 && x != n - 1) return true;
        x = y;
    }
    return x != 1;
}
bool MillerRabin(long long n, long long s) {
    if(n == 2 || n == 3 || n == 5) return true;
    if(n % 2 == 0 || n % 3 == 0 || n % 5 == 0 || n == 1) return false;
    while(s --){
        if(Witness(Rand() % (n - 1) + 1, n)) return false;
    }
    return true;
}
```