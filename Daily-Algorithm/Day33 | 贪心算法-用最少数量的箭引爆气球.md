## 用最少数量的箭引爆气球
### 链接：[https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/submissions/739727839/]
### 解法
首先需要对气球的起始位置进行排序，按起始位置从小到大排好序，然后遍历数组：
如果当前气球的起始位置大于上一个气球的结束位置，说明无法产生交集，必须多用一支箭；
如果当前气球的起始位置小于等于上一个气球的结束位置，说明可以用一支箭同时引爆，不用添加箭。但是因为用同一支箭引爆了，需要将被引爆表达在数组中，以免已经被引爆的气球干扰后面气球的引爆，需要将当前被引爆的气球的结束位置缩短（见代码）
### 代码
```
class Solution {
public:
    static bool cmp(const vector<int> &a, const vector<int> &b){
        return a[0] < b[0];
    }
    int findMinArrowShots(vector<vector<int>>& points) {
        if(points.size() == 0) return 0;
        sort(points.begin(),points.end(),cmp);
        int result = 1;

        for(int i = 1;i < points.size();i++){
            if(points[i][0] > points[i - 1][1]) result++;
            else{
                points[i][1] = min(points[i][1],points[i-1][1]);
            }
        }
        return result;
    }
};
```
