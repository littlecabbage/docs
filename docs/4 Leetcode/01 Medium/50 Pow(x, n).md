#快速幂

![](FigureBed%20🌄/Pasted/Pasted%20image%2020220604213557.png)


```cpp
class Solution {
public:

    double fastPow(double x, long long n){
        double rst = 1.0;
        double base = x;
        while(n){
            if(n & 1) rst *= base;
            
            n >>= 1;
            base *= base;
        }

        return rst;
    }
    double myPow(double x, int n) {
        long long N = (long long)n;
        return n>0?fastPow(x, N):1.0 / fastPow(x, -N);
    }
};
```