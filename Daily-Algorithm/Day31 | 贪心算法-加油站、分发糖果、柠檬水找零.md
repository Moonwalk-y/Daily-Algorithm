## 加油站
### 链接：[https://leetcode.cn/problems/gas-station/]

### 解法
采用贪心算法，使用`start`记录起点的索引，`cursum`记录当前的剩余油量，`totalsum`记录总的一圈的油量。这样当一圈结束后，如果totalsum小于0，那么不管哪里是起点都不可能绕完一圈。
遍历所有加油站，当cursum小于0了，说明当前的start不满足要求，`start = i + 1`，然后继续循环。
之所以`start = i + 1`是因为在i的时候cursum小于0了，那么在原来的start到i的任意一点，最远都超不过i，因为前面每多走一格，这一格带来的都是正收益（大于等于0），比原start靠后但先于i的点获得的正收益都比原start更少，就更不可能是正确的起点了，因此start需要i + 1.

### 代码
```
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int start = 0;
        int cursum = 0;
        int totalsum = 0;
        for(int i = 0;i < gas.size();i++){
            cursum += gas[i] - cost[i];
            totalsum += gas[i] - cost[i];

            if(cursum < 0){
                start = i + 1;
                cursum = 0;
            }
        }

        if(totalsum < 0) return -1;
        return start;
    }
};
```

---
## 分发糖果
### 链接：[https://leetcode.cn/problems/candy/description/]

### 解法
贪心算法，但是要拆分成两个规则：左规则和右规则。左规则是从左到右遍历，如果右边的比左边的高，就比左边的多一个糖果；右边同理。

### 代码
```
class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int> candys(ratings.size(),1);
        for(int i = 1;i < ratings.size();i++){
            if(ratings[i] > ratings[i - 1]) candys[i] = candys[i - 1] + 1;
        }
        for(int i = ratings.size() - 2;i >= 0;i--){
            if(ratings[i] > ratings[i + 1]) candys[i] = max(candys[i],candys[i + 1] + 1);
        }
        int res = 0;
        for(int &p : candys){
            res += p;
        }
        return res;
    }
};
```

---
## 柠檬水找零
### 链接：[https://leetcode.cn/problems/lemonade-change/description/]
这个简单，就不写题解了
