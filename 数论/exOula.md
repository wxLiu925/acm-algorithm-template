```cpp
namespace exoula {
    //求欧拉函数
    long long getOula_function(long long _mod){
        long long i, sum = _mod;
        for(i = 2; i * i <= _mod; i++) {
            if(!(_mod % i)) sum = (long long)sum * (1.0 - 1.0 / i);
            while(!(_mod % i)) _mod /= i;
        }
        if(_mod != 1) sum = (long long)sum * (1.0 - 1.0 / _mod);
        return sum;
    }
    //求一个新的 b 来计算 a^b(mod p)
    long long getDivisor_function(string _str, long long _oula) {
        char c; bool flag = false;
        long long sum=0;
        stringstream _stream;
        _stream << _str;
        while(_stream >> c) {
            sum = 10ll * sum + c - '0';
            if(sum > _oula) {sum %= _oula; flag = true;}
        }
        _stream.clear();
        if(flag) sum += _oula;
        return sum;
    }
    //快速幂
    long long qsm(long long _a, long long _b, long long _mod) {
        _a %= _mod;
        long long sum = 1;
        while(_b > 0) {
            if(_b & 1) sum = sum * _a % _mod;
            _a = _a * _a % _mod;
            _b = _b >> 1;
        }
        return sum;
    }
    //计算 a ^ b % p a是_x b是_str
    long long pow_bigmod(long long _x, string _str, long long _mod) {
        long long _oula = getOula_function(_mod);
        long long _divisor = getDivisor_function(_str, _oula);
        return qsm(_x, _divisor, _mod) % _mod;
    }
}
```