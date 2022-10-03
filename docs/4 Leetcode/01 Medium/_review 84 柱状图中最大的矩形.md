---
Author: sync
date: 2022-06-25 21:05 Saturday
tag: review 单调栈
---

![](FigureBed%20🌄/Pasted/Pasted%20image%2020220625210525.png)


[单调栈入门](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/solution/84-by-ikaruga/)

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> s;
        int ans = 0;

		// 这里为什么要插 0 呢
        heights.insert(heights.begin(), 0);
        heights.push_back(0);

        for(int i = 0 ;i < heights.size(); i++){
            while(!s.empty() && heights[i] < heights[s.top()]){
                int cur = s.top();

                s.pop();
                int left = s.top() + 1;
                int right = i;
                ans = max(ans, (right - left) * heights[cur]);
            }

            s.push(i);
        }

        return ans;
    }
};
```