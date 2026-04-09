## 递增子序列
[https://programmercarl.com/0491.%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97.html]

#### 解法
```
class Solution {
private:
    vector<int> group;
    vector<vector<int>> res;
public:
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        vector<int> used(nums.size(),0);
        backTravel(nums,0);
        return res;
    }
    void backTravel(vector<int>& nums,int index){
        if(group.size() >= 2) res.push_back(group);
        vector<int> used(201,0);
        for(int i = index;i < nums.size();i++){
            if(!group.empty() && group[group.size() - 1] > nums[i]) continue;
            if(used[nums[i] + 100] == 1) continue;
            group.push_back(nums[i]);
            used[nums[i] + 100] = 1;
            backTravel(nums,i + 1);
            group.pop_back();
        }
        return;
    }
};
```
---
## 全排列
[https://programmercarl.com/0046.%E5%85%A8%E6%8E%92%E5%88%97.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

#### 解法
注意，回溯的调用有时需要包裹在条件中。而且像这里，count+1写在调用的过程中，就不需要在回溯过程中count--了，因为原函数的count值其实没有改变。
```
class Solution {
private:
    vector<int> group;
    vector<vector<int>> res;
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<int> used(nums.size(),0);
        backTravel(nums,used,0);
        return res;
    }
    void backTravel(vector<int> &nums,vector<int> &used,int count){
        if(count == nums.size()){
            res.push_back(group);
            return;
        }
        for(int i = 0;i < nums.size();i++){
            if(used[i] != 1){
                group.push_back(nums[i]);
                used[i] = 1;
                backTravel(nums,used,count + 1);
                group.pop_back();
                used[i] = 0;
            }
        }
        return;
    }
};
```
---
## 全排列Ⅱ
[https://programmercarl.com/0047.%E5%85%A8%E6%8E%92%E5%88%97II.html]

####
```
class Solution {
private:
    vector<int> group;
    vector<vector<int>> res;
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vector<int> used(nums.size(),0);
        backTravel(nums,used,0);
        return res;
    }
    void backTravel(vector<int>& nums,vector<int> used,int count){
        if(count == used.size()){
            res.push_back(group);
            return;
        }
        for(int i = 0;i < nums.size();i++){
            if(i > 0 && nums[i] == nums[i - 1] && used[i - 1] == 0) continue;
            if(used[i] == 0){
                group.push_back(nums[i]);
                used[i] = 1;
                backTravel(nums,used,count + 1);
                used[i] = 0;
                group.pop_back();
            }
        }
        return;
    }
};
```
