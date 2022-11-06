#算法/脑筋急转弯 

![](FigureBed%20🌄/Pasted/Pasted%20image%2020220608155717.png)

```cpp
class Solution {
public:
    int rand10() {
        int row, col, idx;
        do {
            row = rand7();
            col = rand7();
            idx = col + (row - 1) * 7;
        } while (idx > 40);
        return 1 + (idx - 1) % 10;
    }
};
```