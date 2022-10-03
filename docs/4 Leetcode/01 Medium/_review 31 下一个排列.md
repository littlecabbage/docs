---
Author: sync
date: 2022-06-13 09:27 Monday
tag: review
date updated: 2022-10-03 22:11 Monday
---

#脑筋急转弯
![[FigureBed%20🌄/Pasted/Pasted%20image%2020220605213343.png]]

```cpp
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int i = nums.size() - 2;
        while (i >= 0 && nums[i] >= nums[i + 1]) {
            i--;
        }
        if (i >= 0) {
            int j = nums.size() - 1;
            while (j >= 0 && nums[i] >= nums[j]) {
                j--;
            }
            swap(nums[i], nums[j]);
        }
        reverse(nums.begin() + i + 1, nums.end());
    }
};

```
