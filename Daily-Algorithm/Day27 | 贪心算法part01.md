## 分发饼干
[https://programmercarl.com/0455.%E5%88%86%E5%8F%91%E9%A5%BC%E5%B9%B2.html]

#### 解法
```
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(),g.end());
        sort(s.begin(),s.end());
        int res = 0;
        int i = 0;int j = 0;
        while(i < g.size() && j < s.size()){
            if(s[j] >= g[i]){
                res++;
                i++;
            }
            j++;
        }
        return res;
    }
};
```

## 摆动序列
[https://programmercarl.com/0376.%E6%91%86%E5%8A%A8%E5%BA%8F%E5%88%97.html]

#### 解法
```
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int n = nums.size();
        if(n == 1) return 1;
        int count = 1;
        int pre_diff = 0;
        for(int i = 1;i < n;i++){
            int diff = nums[i] - nums[i - 1];
            if((diff < 0 && pre_diff >= 0) || (diff > 0 && pre_diff <= 0)){
                count ++;
                pre_diff = diff;
            }
        }
        return count;
    }
};
```

## 最大子序和
[https://programmercarl.com/0053.%E6%9C%80%E5%A4%A7%E5%AD%90%E5%BA%8F%E5%92%8C.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

#### 解法
这个解法说实话太巧妙了，短短的几行代码就解决了这个看似难的问题。依旧是贪心算法，贪的是如果count<0了，那一定会把后面的值变小，所以直接舍弃重新开始以下一个为开始。
```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int res;
        int count = 0;
        for(int i = 0;i < nums.size();i++){
            count += nums[i];
            if(count > res) res = count;
            if(count < 0) count = 0;
        }
        return res;
    }
};
```
