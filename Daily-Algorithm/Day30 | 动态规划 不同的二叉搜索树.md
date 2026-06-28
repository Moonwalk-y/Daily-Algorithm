题目链接：[96. 不同的二叉搜索树 - 力扣（LeetCode）](https://leetcode.cn/problems/unique-binary-search-trees/)
### 解法
二叉搜索树一旦形状确定，其内部节点的填充方法就是唯一确定的（从下方投影看，左到右依次增大），所以这题就是求不同形状的二叉搜索树数量，进而转换成左子树形状种类 * 右子树形状种类

dp数组：`dp[i]`表示节点数为i时的形状数量

初始化： 0、1时为1，2时为2

递推关系：设一个j，表示头节点在第j个位置，j从1到i。`dp[i] += dp[j - 1] * dp[i - j]`。意思是左边的可能数量乘右边的可能数量

### 代码
```
class Solution {
public:
    int numTrees(int n) {
        if(n == 1) return 1;
        if(n == 2) return 2;
        vector<int> dp(n + 1,0);
        dp[0] = dp[1] = 1;
        dp[2] = 2;
        for(int i = 3;i <= n;i++){
            for(int j = 1;j <= i;j++){
                dp[i] += dp[j-1] * dp[i - j];
            }
        }
        return dp[n];
    }
};
```
