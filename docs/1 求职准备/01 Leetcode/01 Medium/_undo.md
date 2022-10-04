---
Author: sync
date: 2022-06-26 10:42 Sunday
tag: undo 
---
![](FigureBed%20🌄/Pasted/Pasted%20image%2020220626104413.png)


```cpp
class Solution {
public:
    void rotateElement(int cur, int top, int bottom, int left, int right, vector<vector<int>>& matrix){
        swap(matrix[cur][left], matrix[bottom][cur]);
        swap(matrix[bottom][cur], matrix[cur][right]);
        swap(matrix[cur][right], matrix[top][cur]);     
    }
    void rotate(vector<vector<int>>& matrix) {
        int row = matrix.size(), col = matrix[0].size();

        int left, right, top, bottom;
        left = top = 0;
        bottom = row - 1;
        right = col - 1;

        while(left < right && top < bottom){
            for(int cur = top; cur < bottom; cur++){
                rotateElement(cur, top, bottom, left, right, matrix);
            }

            top++;bottom--;
            left++; right--;
        }
    }
};
```