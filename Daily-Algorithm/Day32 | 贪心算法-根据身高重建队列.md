## 根据身高重建队列
### 链接：[https://leetcode.cn/problems/queue-reconstruction-by-height/]
### 解法
当一遍遍历无法解决问题时，就要考虑从两个维度来解决。该题的两个维度一个是身高，一个就是位置。
首先，将身高从高到低进行排序，如果身高相同，索引小的在前面。
然后从高到低进行遍历，此时他的索引就是他应该在的位置，使用insert将其插入。
之所以这样做，是因为如果一个人前面有3个身高大于等于他的人，那么他一定在索引为3（也就是第四个）的位置上，因为经过排序，他前面的人一定不低于他
### 代码
```
class Solution {
public:
    static bool cmp(const vector<int> &a, const vector<int> &b){
        if(a[0] == b[0]) return a[1] < b[1];
        return a[0] > b[0];
    }
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(),people.end(),cmp);
        vector<vector<int>> que;
        int position = 0;
        for(int i = 0;i < people.size();i++){
            position = people[i][1];
            que.insert(que.begin() + position, people[i]);
        }
        return que;
    }
};
```
### 补充
#### sort的用法
需要一个对比函数，如果`return a > b`，那么a在b的前面。
如果 a 的身高 > b 的身高，返回 true → 高个子排在前面
如果 a 的身高 < b 的身高，返回 false → 矮个子排在后面
#### insert的用法
- 工作原理：
  检查容量：如果空间不足，先扩容（重新分配内存）
移动元素：从插入位置开始，所有元素向后移动一位
插入元素：将新元素复制/移动到指定位置
更新大小：size() 增加 1
