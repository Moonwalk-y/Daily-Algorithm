## 不同路径Ⅱ
### 链接
[63. 不同路径 II - 力扣（LeetCode）](https://leetcode.cn/problems/unique-paths-ii/submissions/741215691/)
### 解法
和不同路径Ⅰ不同的是，这里有一些石头会堵住路。因此在初始化和递推公式上需要有一定的改变。
首先，初始化依然是初始化最左和最上的行列，但是如果遇到石头了，后面的就不初始化了，因为无法到达。
递推公式上，如果当前格子有石头，那么到达方法就是0，直接跳过。其他的都一样
### 代码
```C++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        vector<vector<int>> dp(m,vector<int>(n,0));
  
        for(int i = 0;i < n && obstacleGrid[0][i] != 1;i++) dp[0][i] = 1;
        for(int i = 0;i < m && obstacleGrid[i][0] != 1;i++) dp[i][0] = 1;
  
        for(int i = 1;i < m;i++){
            for(int j = 1;j < n;j++){
                if(obstacleGrid[i][j] == 1) continue;
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m-1][n-1];
    }
};
```

---
## 整数拆分
### 链接
[343. 整数拆分 - 力扣（LeetCode）](https://leetcode.cn/problems/integer-break/description/)
### 解法
dp数组定义：`dp[i]`表示i的最大拆分结果
初始化：`dp[1] = 1 dp[2] = 2 dp[3] = 3`。之所以`dp[3] != 2`，是因为选择拆分结果和自身这两个里面最大的一个，否则会出现4 = 2 + 2，然后`dp[4] = dp[2] * dp[2] = 1`的情况.
遍历顺序：i从前到后，用j来分割i，j <= i / 2即可
递推公式：`dp[i] = max(dp[i],dp[i - j] * dp[j])`
### 代码
```C++
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n + 1,0);
        if(n == 2) return 1;if(n == 3) return 2;
        dp[1] = 1;
        dp[2] = 2;
        dp[3] = 3;
        for(int i = 4;i <= n;i++){
            for(int j = 1;j <= i / 2;j++){
                dp[i] = max(dp[i],dp[i - j] * dp[j]);
            }
        }
        return dp[n];
    }
};
```