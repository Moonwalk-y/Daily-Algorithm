题目链接：[63. 不同路径 II - 力扣（LeetCode）](https://leetcode.cn/problems/unique-paths-ii/description/)]

### 解法
使用动态规划解决。

首先dp数组为二维数组，定义是到达第i行j列格子的可能方法数

递推公式就是`dp[i][j] = dp[i - 1][j] + dp[i][j - 1];`，就是要注意，如果该格子有障碍物，那么就不需要算了，直接设为0

dp数组的初始化就是将第一行和第一列都设为1，但是注意：如果第一行或者第一列有障碍，那么障碍格子以及之后的到达方法都是0，判断可以写在for循环中，就不需要用break了，很巧妙

遍历顺序是从左到右从上到下，因为递推公式依赖左边和上边的数据
### 代码
```
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        vector<vector<int>> dp(m,vector<int>(n,0));
        for(int i = 0;i < m && obstacleGrid[i][0] == 0;i++){dp[i][0] = 1;}
        for(int j = 0;j < n && obstacleGrid[0][j] == 0;j++){dp[0][j] = 1;}
  
        for(int i = 1;i < m;i++){
            for(int j = 1;j < n;j++){
                if(obstacleGrid[i][j] == 0)
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m-1][n-1];
    }
};
```
