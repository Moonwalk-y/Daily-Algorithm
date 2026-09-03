## 零钱兑换
### 链接
[322. 零钱兑换 - 力扣（LeetCode）](https://leetcode.cn/problems/coin-change/description/)
### 解法
这里要求的是最小的值，还是完全背包问题。dp数组不用多说。
初始化：amount是0的话最小就是0，其他的都要为max，如果初始化为0的话会覆盖正确数据
遍历顺序：一般情况都是先物品再容量
dp数组：`dp[j] = min(dp[j],dp[j - coins[i]] + 1)`，要么选当前的硬币，要么不选，选的话就+1.
如果最后的`dp[amount] == MAX`,说明凑不出来该金额。
其实和求最大的没啥不一样，只是0变MAX，max变min。
### 代码
```C++
class Solution {
#define MAX 214748364
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount+1,MAX);
        dp[0] = 0;
        for(int i = 0;i < coins.size();i++){
            for(int j = 1;j <= amount;j++){
                if(j >= coins[i]) dp[j] = min(dp[j],dp[j - coins[i]] + 1);
            }
        }
        if(dp[amount] == MAX) return -1;
        return dp[amount];
    }
};
```