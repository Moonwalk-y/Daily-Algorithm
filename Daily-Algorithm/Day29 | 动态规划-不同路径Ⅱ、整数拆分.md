## 不同路径Ⅱ
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
## 整数拆分
题目链接：[343. 整数拆分 - 力扣（LeetCode）](https://leetcode.cn/problems/integer-break/)
### 解法
使用动态规划解决

dp数组的定义为`dp[i]`是数字i的拆分最大乘积

递推关系是`dp[i] = max(j * (i - j),j * dp[i - j],dp[i])`，其中`j * (i - j)`的意思是拆成两个数，`j * dp[i - j]`的意思是拆成三个即以上的数，`dp[i - j]`的意思就是另一个数的最大乘积。其中`dp[i]`也参与对比是因为i固定的情况下，`dp[i]`会多次参与对比并更新，要保留过程中的最大值

dp数组初始化，索引为0和1的话都是0，因为无法拆分。索引为2最大是1

遍历顺序从左到右，外层循环i从3到n + 1，用于更新dp，内存循环j从1到i，用来拆分数
### 代码
```
class Solution {
public:
    int integerBreak(int n) {
        if(n < 3) return 1;
        vector<int> dp(n + 1);
        dp[0] = dp[1] = 0;
        dp[2] = 1;
        for(int i = 3;i < n + 1;i++){
            for(int j = 1;j < i;j++){
                int tmp = max(j * (i - j),j * dp[i - j]);
                dp[i] = max(tmp,dp[i]);
            }
        }
        // for(int i : dp){
        //     cout << i << " ";
        // }
        return dp[n];
    }
};
```
