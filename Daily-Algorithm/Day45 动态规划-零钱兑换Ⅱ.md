## 零钱兑换Ⅱ
### 链接
[518. 零钱兑换 II - 力扣（LeetCode）](https://leetcode.cn/problems/coin-change-ii/description/)
### 解法
每个硬币可以无限使用，属于完全背包问题
dp数组：物品是硬币种类，背包容量是金额，`dp[i][j]`表示用前i种硬币凑成j的最大组合数
初始化：j为0时，有1种方法，就是都不选。i为0时，看j能不能被面额整除，能的话就是1，否则是0
遍历顺序：正序，这样可以重复使用
递推公式：`dp[i][j] = dp[i-1][j] + dp[i][j - coins[i]]`，意思是要么不用当前的币，要么用当前的币。注意，如果用当前的币，索引为i不是i-1，因为可以重复使用，语义是现在至少用一张当前币。注意不是i-1的话，是没有退回上一层的，还是在当前层
### 代码
```C++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        if(amount == 0) return 1;
        vector<vector<unsigned long long>> dp(coins.size(),vector<unsigned long long>(amount+1,0));
        for(int i = 1;i <= amount;i++){
            if(i % coins[0] == 0) dp[0][i] = 1;
        }
        for(int i = 0;i < coins.size();i++){
            dp[i][0] = 1;
        }
        for(int i = 1;i < coins.size();i++){
            for(int j = 1;j <= amount;j++){
                if(j >= coins[i]){
                    dp[i][j] = dp[i-1][j] + dp[i][j - coins[i]];
                }
                else dp[i][j] = dp[i-1][j];
            }
        }
        return dp[coins.size()-1][amount];
    }
};
```