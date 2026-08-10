## 使用最小花费爬楼梯
### 链接
[746. 使用最小花费爬楼梯 - 力扣（LeetCode）](https://leetcode.cn/problems/min-cost-climbing-stairs/solutions/528955/shi-yong-zui-xiao-hua-fei-pa-lou-ti-by-l-ncf8/)
### 解法
dp数组定义为爬到第i个台阶需要的最小花费
初始化是`dp[0]和dp[1] = 0`,因为这两个是初始可以站在上面的台阶，无花费。
因为每次能爬一层或两层，
所以递推公式：`dp[i] = min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2])`
遍历从前到后。
### 代码
```C++
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n = cost.size();
        vector<int> dp(n + 1);
  
        dp[0] = 0;
        dp[1] = 0;
  
        for(int i = 2;i <= n;i++){
            dp[i] = min(dp[i - 1] + cost[i - 1],dp[i - 2] + cost[i - 2]);
        }
  
        return dp[n];
    }
};
```

---
## 不同路径
### 链接
[62. 不同路径 - 力扣（LeetCode）](https://leetcode.cn/problems/unique-paths/description/)
### 解法
使用一个二维的dp数组，含义是到达第i行j列的格子有几种方法。
递推公式简单，不列出来了
### 代码
```C++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m + 1,vector<int>(n + 1));
        for(int i = 0;i <= m;i++) dp[i][0] = 1;
        for(int i = 0;i <= n;i++) dp[0][i] = 1;
  
        for(int i = 1;i < m;i++){
            for(int j = 1;j < n;j++){
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
  
        return dp[m-1][n-1];
    }
};
```