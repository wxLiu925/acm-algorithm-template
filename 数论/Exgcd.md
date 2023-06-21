```cpp
class EXGCD {
public:
    struct node {
        int g, x, y;
    };
    int a, b, c; // ax + by = c
    EXGCD(int a, int b, int c): a(a), b(b), c(c) { }
    // 解 ax + by = gcd(a, b)
    int exgcd(int aa, int bb, int &x, int &y) {
        if(!bb) {
            x = 1, y = 0;
            return aa;
        }
        int d = exgcd(bb, aa % bb, y, x);
        y -= (aa / bb) * x;
        return d;
    }
    // 解 ax + by = c
    node Exgcd() {
        int g = std::__gcd(a, b);
        if(c % g != 0) return {-1, 0, 0}; // 无解
        int x, y;
        exgcd(a, b, x, y); // 先求 ax + by = gcd(a, b)
        int t = c / g;
        x = x * t, y = y * t;
        return {g, x, y};
    }
    int exgcd2(int a, int b, int &x, int &y) {
        if(b == 0) {
            x = 1, y = 0;
            return a;
        }
        int d = exgcd(b, a % b, x, y);
        int z = x;
        x = y, y = z - a / b * y;
        return d;
    }
    node Exgcd_w() {
        int x, y, g;
        g = exgcd2(b, a, x, y); 
        if(c % g != 0) return {-1, 0, 0};
        int x0 = a / g, y0 = b / g; 
        y = ((c / g % x0) * (x % x0) % x0 + x0) % x0;
        x = (c - y * b) / a;
        return {g, x, y};
    }
    // 求 ax + by = c 关于 x 的最小正整数解, 这时候也是 y 的最大值
    node Exgcd_X() {
        auto res = Exgcd();
        if(res.g == -1) return res; // 无解
        int &x = res.x, &y = res.y, &g = res.g;
        int tx = b / g, ty = a / g;
        int k = ceil((1.0 - x) / tx); // 求k
        x += tx * k;
		y -= ty * k;
        return res;
    }
    // 求 ax + by = c 关于 y 的最小正整数解, 这时候也是 x 的最大值
    node Exgcd_Y() {
        auto res = Exgcd();
        if(res.g == -1) return res; // 无解
        int &x = res.x, &y = res.y, &g = res.g;
        int tx = b / g, ty = a / g;
        int k = ceil((1.0 - y) / ty); // 求k
        x -= tx * k;
		y += ty * k;
        return res;
    }
    // 求正整数解的数量
    int Exgcd_cnt() {
        auto res = Exgcd_X();
        if(res.g == -1) return 0;
        int tx = b / res.g, ty = a / res.g;
        int k = ceil((1.0 - res.x) / tx); // 求k
        return ((res.y - 1) / ty + 1);
    }
};
```