## 无重叠区间
### 链接
[https://leetcode.cn/problems/non-overlapping-intervals/description/]
### 解法
贪心算法，先按起点从小到大排序，如果起点一样按终点从小到大排。然后遍历，如果发现有重叠区间，将后一个区间的终点修改成前一个区间终点以及自己之间的最小值，相当于删除了一个区间。

### 代码
```C++
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
先设置一个字母表，循环一遍用于记录每个字母最后出现的位置。
然后用start和end记录区间的起止。从头开始循环，end取原来的值和当前字母最后出现的位置中最大的一个，是新的区间结束点。当i走到了一个区间的结束，记录结果，然后更新start

之所以这样做，是因为一个字母第一次出现和最后一次出现一定是在同一个区间里，所以可以确定下来end

### 代码
```C++
class Solution {
public:
    vector<int> partitionLabels(string s) {
        int last[26];
        for(int i = 0;i < s.size();i++){
            last[s[i] - 'a'] = i;
        }
  
        vector<int> result;
        int start = 0;
        int end = 0;
        for(int i = 0;i < s.size();i++){
            end = max(end, last[s[i] - 'a']);
            if(i == end){
                result.push_back(end - start + 1);
                start = i + 1;
            }
        }
        return result;
    }
};
```
