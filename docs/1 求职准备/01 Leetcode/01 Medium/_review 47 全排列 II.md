---
Author: sync
date: 2022-06-13 19:26 Monday
tag: 算法/review
---

#算法/回溯

有问题的代码

```cpp
class Solution {
public:
    vector<vector<int>> res;
    
    void dfs(vector<int> &path, vector<int> &vis, int n, vector<int> nums){
        if(path.size() == n){
            res.push_back(path);
            return;
        }

        for(int i = 0; i < path.size(); i++){
            cout << path[i] << " ";
        }
        cout << endl;
    
        for(int i = 0 ; i < n ;i ++){
            if(i == 0 && !vis[i]){
                vis[i] = 1;
                path.push_back(i);
                dfs(path, vis, n, nums);
                path.pop_back();
                vis[i] = 0;
            }else{
                if(nums[i] != nums[i-1] && !vis[i]){
                    vis[i] = 1;
                    path.push_back(i);
                    dfs(path, vis, n, nums);
                    path.pop_back();
                    vis[i] = 0;
                }
            }
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<int> path;
        vector<int> vis(nums.size(), 0);
        dfs(path, vis, nums.size(), nums);
        return res;
    }
};
```

```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    void dfs(vector<int>& nums, vector<int> vis, int len){
        if(path.size() == len){
            res.push_back(path);
            return;
        }

        for(int i = 0; i < nums.size(); i++){
            
            if(i > 0 && nums[i] == nums[i-1] && vis[i-1]) continue;
            
            if(!vis[i]){
                vis[i] = 1;
                path.push_back(nums[i]);
                dfs(nums, vis, len);
                vis[i] = 0;
                path.pop_back();
            }
        }
    }

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        vector<int> vis(n, 0);
        dfs(nums, vis, n);
        return res;
    }
};
```
