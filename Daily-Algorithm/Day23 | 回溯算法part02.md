## 组合总和
[https://programmercarl.com/0039.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8C.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

#### 解法
如果允许使用重复数字的话递归时就不要把索引加一了，而是传原来的索引。但是这样就有用终止条件来避免无限递归：当sum大于target的时候continue（不能break不然有些情况会错过正确组合）。
```
class Solution {
private:
    vector<int> group;
    vector<vector<int>> res;
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        backTravel(candidates,target,0,0);
        return res;
    }
    void backTravel(vector<int> &candidates,int target,int index,int sum){
        if(index == candidates.size()) return;
        if(sum == target){
            res.push_back(group);
            return;
        }
        for(int i = index;i < candidates.size();i++){
            if(sum + candidates[i] > target) continue;
            group.push_back(candidates[i]);
            backTravel(candidates,target,i,sum + candidates[i]);
            group.pop_back();
        }
    }
};
```
---
## 组合总和Ⅱ
[https://programmercarl.com/0040.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CII.html]

#### 解法
```

```
---
## 
[]

#### 解法
```

```
---
