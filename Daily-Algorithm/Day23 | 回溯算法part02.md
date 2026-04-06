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
需要在树层上进行去重，使用used数组记录元素是否被使用过。如果该元素与上一个元素相等，但上一个元素的used记录是使用过，那说明是在同一个数枝上，这样是允许的；如果上一个元素没被使用过，那说明这个重复是树层上的，不被允许，直接continue。
```
class Solution {
private:
    vector<int> group;
    vector<vector<int>> res;
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(),candidates.end());
        vector<int> used(candidates.size(),0);
        BackTravel(candidates,target,0,0,used);
        return res;
    }
    void BackTravel(vector<int>& candidates,int target,int index,int sum,vector<int>& used){
        if(sum > target) return;
        if(sum == target){
            res.push_back(group);
            return;
        }
        for(int i = index;i < candidates.size();i++){
            if(i > 0 && candidates[i] == candidates[i - 1] && used[i-1] == 0) continue;
            group.push_back(candidates[i]);
            sum += candidates[i];
            used[i] = 1;
            BackTravel(candidates,target,i+1,sum,used);
            group.pop_back();
            sum -= candidates[i];
            used[i] = 0;
        }
    }
};
```
---
## 分割回文串
[https://programmercarl.com/0131.%E5%88%86%E5%89%B2%E5%9B%9E%E6%96%87%E4%B8%B2.html#%E6%80%9D%E8%B7%AF]

#### 解法
很有意思，这道题竟然也能用到回溯算法。index算是分割的隔板，index之前的是已经加入到path里的子串，后面用i去找符合条件的子串。其中每划分出一个子串，就用函数判断是不是回文段，不是直接continue结束这层for循环，因为这种分割方式不可能出现符合条件的情况了；如果是回文段，加入path。这样当index挪动到最后，得到的path就已经是符合条件的情况。不需要在判断path是不是都是回文子串了
```
class Solution {
private:
    vector<string> path;
    vector<vector<string>> res;
public:
    vector<vector<string>> partition(string s) {
        backtravel(s,0);
        return res;
    }
    void backtravel(string &s,int index){
        if(index >= s.size()){
            res.push_back(path);
            return;
        }
        for(int i = index;i < s.size();i++){
            if(ishuiwen(s,index,i)){
                path.push_back(s.substr(index,i - index + 1));
            }else{
                continue;
            }
            backtravel(s,i + 1);
            path.pop_back();
        }
        return;
    }
    bool ishuiwen(string &s,int start,int now){
        if(start == now) return true;
        int i = start;
        int j = now;
        while(i < j){
            if(s[i] != s[j]) return false;
            i++;j--;
        }
        return true;
    }
};
```
---
