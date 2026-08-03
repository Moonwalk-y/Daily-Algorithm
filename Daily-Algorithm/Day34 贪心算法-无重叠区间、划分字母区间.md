## 无重叠区间
### 链接
[https://leetcode.cn/problems/non-overlapping-intervals/description/]
### 解法
### 代码
```
class Solution {
public:
    static bool cmp(const vector<int> &a, const vector<int> &b){
        if(a[0] == b[0]) return a[1] < b[1];
        return a[0] < b[0];
    }
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end(),cmp);
        int result = 0;
        for(int i = 1;i < intervals.size();i++){
            if(intervals[i][0] < intervals[i - 1][1]){
                intervals[i][1] = min(intervals[i][1],intervals[i - 1][1]);
                result++;
            }
        }
        return result;
    }
};
```

---
## 划分字母区间
### 链接
[https://leetcode.cn/problems/partition-labels/description/]
### 解法
### 代码
```
```
