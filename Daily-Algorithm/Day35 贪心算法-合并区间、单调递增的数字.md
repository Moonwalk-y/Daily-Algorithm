## 合并区间
### 链接
[56. 合并区间 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-intervals/solutions/203562/he-bing-qu-jian-by-leetcode-solution/)
### 解法
依旧是先排序，然后发现有重叠的了，就把重叠的这个的结束更新，然后更新end。
如果没重叠，那么就把start和end放到结果数组里，然后更新start和end。

### 代码
```C++
class Solution {
public:
    static bool cmp(const vector<int> &a, const vector<int> &b){
        return a[0] < b[0];
    }
  
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
       sort(intervals.begin(),intervals.end(),cmp);
        vector<vector<int>> result;
        int start = intervals[0][0];
        int end = intervals[0][1];
        if(intervals.size() == 1){
            result.push_back({start,end});
            return result;
        }
        for(int i = 1;i < intervals.size();i++){
            if(intervals[i][0] <= intervals[i - 1][1]){
                intervals[i][1] = max(intervals[i][1],intervals[i - 1][1]);
                end = intervals[i][1];
            }
            else{
                cout << start << " " << end << endl;
                result.push_back({start,end});
                start = intervals[i][0];
                end = intervals[i][1];
            }
        }
        result.push_back({start,end});
        return result;
    }
};
```

---
## 单调递增的数字
### 链接
[738. 单调递增的数字 - 力扣（LeetCode）](https://leetcode.cn/problems/monotone-increasing-digits/)
### 解法
从后往前遍历，因为从前往后的话会让数字变乱。
如果遇到当前数字比前一个大，就设成9，然后上一个数字-1。可能需要重复该过程，就用一个flag来标致从当前位置往后应该都是9，最后加一个for循环用来置9

### 代码
```C++
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        string num = to_string(n);
        if(num.size() == 1) return n;
        int flag;
        for(int i = num.size() - 1;i > 0;i--){
            if(num[i - 1] > num[i]){
                flag = i;
                num[i - 1]--;
            }
        }
        for(int i = flag;i < num.size();i++){
            num[i] = '9';
        }
        return stoi(num);
    }
};
```