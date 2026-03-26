## 组合
[https://programmercarl.com/0077.%E7%BB%84%E5%90%88.html#%E6%80%9D%E8%B7%AF]

#### 解法
```
class Solution {
private:
    vector<vector<int>> res;
    vector<int> path;
public:
    vector<vector<int>> combine(int n, int k) {
        backTravel(n,k,1);
        return res;
    }
    void backTravel(int n,int k,int index){
        if(path.size() == k){
            res.push_back(path);
            return;
        }
        for(int i = index;i <= n;i++){
            path.push_back(i);
            backTravel(n,k,i + 1);
            path.pop_back();
        }
    }
};
```

---
## 组合总和Ⅲ
[https://programmercarl.com/0216.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CIII.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

#### 解法
```
class Solution {
private:
    vector<int> path;
    vector<vector<int>> res;
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        backTravel(k,n,1,0);
        return res;
    }
    void backTravel(int k,int n,int index,int sum){
        if(path.size() > k || sum > n) return;
        if(path.size() == k && sum < n) return;
        if(path.size() == k && sum == n){
            res.push_back(path);
            return;
        }
        for(int i = index;i <= 9;i++){
            path.push_back(i);
            backTravel(k,n,i + 1,sum + i);
            path.pop_back();
        }
    }
};
```

---
## 电话号码的字母组合
[https://programmercarl.com/0017.%E7%94%B5%E8%AF%9D%E5%8F%B7%E7%A0%81%E7%9A%84%E5%AD%97%E6%AF%8D%E7%BB%84%E5%90%88.html#%E6%80%9D%E8%B7%AF]

#### 解法
这一题相较前两题有一点麻烦，因为多了一步，在给定的数字组合上延申出数字对应的字母组合。其实还是分层，因为给几个数字k就是多少，比如给三个数字就是三个字符串进行组合。这样第一层是第一个数，第二层是第二个数，以此类推。一层处理一个对应的字符串就可以。不像前面是在同一个字符串上处理，要不断缩短字符串。
```
class Solution {
private:
    string group;
    vector<string> res;
    vector<string> arr = {"","","abc","def","ghi","jkl","mno","pqrs","tuv","wxyz"};
public:
    vector<string> letterCombinations(string digits) {
        if(digits.size() == 0) return res;
        backTravel(digits,0);
        return res;
    }
    void backTravel(string digits,int index){
        if(index == digits.size()){
            res.push_back(group);
            return;
        }
        int num = digits[index] - '0';
        string chars = arr[num];
        for(int i = 0;i < chars.size();i++){
            group += chars[i];
            backTravel(digits,index+1);
            group.pop_back();
        }
    }
};
```
