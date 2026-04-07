## 复原IP地址
[https://programmercarl.com/0093.%E5%A4%8D%E5%8E%9FIP%E5%9C%B0%E5%9D%80.html#%E6%80%9D%E8%B7%AF]

### 解法
和分割回文串有点像，思路相似，但是有一点要注意，在本题中，for循环中如果遇到不符合要求的段，直接break，因为遇到不符合要求的段那就说明不可能合适了。break寻找另一种。
还有，要注意string中的数字判断的时候要使用 '数字' 的方式。
```
class Solution {
private:
    vector<string> res;
public:
    vector<string> restoreIpAddresses(string s) {
        backTravel(s,0,0);
        return res;
    }
    void backTravel(string &s,int index,int point_sum){
        if(point_sum >= 3){
            if(is_valid(s,index,s.size()-1)){
                res.push_back(s);
            }
            return;
        }
        for(int i = index;i < s.size();i++){
            if(is_valid(s,index,i)){
                s.insert(s.begin() + i + 1,'.');
                point_sum++;
                backTravel(s,i + 2,point_sum);
                s.erase(s.begin() + i + 1);
                point_sum--;
            }else break;
        }
        return;
    }
    bool is_valid(string s,int start,int end){
        if(start > end) return false;
        if(end - start + 1 > 3) return false;
        if(s[start] == '0' && end != start) return false;
        int num = 0;
        for(int i = start;i <= end;i++){
            if(s[i] > '9' || s[i] < '0') return false;
            num = num * 10 + (s[i] - '0');
            if(num > 255) return false;
        }
        return true;
    }
};
```

## 子集
[https://programmercarl.com/0078.%E5%AD%90%E9%9B%86.html]

### 解法
```
class Solution {
private:
    vector<int> group;
    vector<vector<int>> res;
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        back_Travel(nums,0);
        return res;
    }
    void back_Travel(vector<int> &nums,int index){
        res.push_back(group);
        if(index == nums.size()) return;
        for(int i = index;i < nums.size();i++){
            group.push_back(nums[i]);
            back_Travel(nums,i + 1);
            group.pop_back();
        }
        return;
    }
};
```

## 子集Ⅱ
[https://programmercarl.com/0090.%E5%AD%90%E9%9B%86II.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

### 解法
去重需排序！
```
class Solution {
private:
    vector<int> group;
    vector<vector<int>> res;
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<int> used(nums.size(),0);
        sort(nums.begin(),nums.end());
        back_Travel(nums,0,used);
        return res;
    }
    void back_Travel(vector<int> &nums,int index,vector<int> &used){
        res.push_back(group);
        if(index == nums.size()) return;
        for(int i = index;i < nums.size();i++){
            if(i > 0 && nums[i] == nums[i - 1] && used[i-1] == 0) continue;
            group.push_back(nums[i]);
            used[i] = 1;
            back_Travel(nums,i + 1,used);
            group.pop_back();
            used[i] = 0;
        }
        return;
    }
};
```
