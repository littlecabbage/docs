---
Author: sync
date: 2022-06-13 09:26 Monday
tag: 算法/undo 
---

#算法/回溯

![](FigureBed%20🌄/Pasted/Pasted%20image%2020220605223840.png)

```cpp
class Solution {
public:
    bool rst = false;
    int row, col;
    string target;
    void dfs(int i, int j, string &temp, vector<vector<int>> &vis, vector<vector<char>> board){
        if(i < 0 || i >= row || j < 0 || j >= col || vis[i][j]) return;

        vis[i][j] = 1;
        temp.push_back(board[i][j]);

        if(temp == target){
            rst = true;
            return ;
        }


        dfs(i, j-1, temp, vis, board);
        dfs(i, j+1, temp, vis, board);
        dfs(i-1, j, temp, vis, board);
        dfs(i+1, j, temp, vis, board);

        vis[i][j] = 0;
        temp.pop_back();
    }
    bool exist(vector<vector<char>>& board, string word) {
        row = board.size();
        col = board[0].size();
        target = word;

        for(int i = 0; i<row; i++){
            for(int j = 0; j < col;j++){
                if(!rst && board[i][j] == word[0]){
                    string temp = "";
                    vector<vector<int>> vis(row, vector<int>(col, 0));
                    dfs(i, j, temp, vis, board);
                }
            }
        }
        return rst;
    }
};

```

超时

```cpp
[["b","b","a","a","b","a"],["b","b","a","b","a","a"],["b","b","b","b","b","b"],["a","a","a","b","a","a"],["a","b","a","a","b","b"]]
"abbbababaa"
```

<mark style="background: #ABF7F7A6;">该模板涉嫌大量无效函数调用</mark> 

# 抄的

```cpp
class Solution {
public:
    vector<vector<int>> res;
    void dfs(vector<int>& path, vector<int>& vis, vector<int> candidates, int start, int target){
        if(target < 0) return ;
        if(target == 0){
            res.push_back(path);
            return ;
        }

        for(int i = start; i < candidates.size(); i++){
            if(i > start && candidates[i] == candidates[i-1]) continue;
            if(vis[i] == 0){
                path.push_back(candidates[i]);
                vis[i] = 1;
                dfs(path, vis, candidates, i+1, target-candidates[i]);
                vis[i] = 0;
                path.pop_back();
            }
        }
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        vector<int> vis(candidates.size(), 0);
        vector<int> path;
        dfs(path, vis, candidates, 0, target);

        return res;
    }
};
```
