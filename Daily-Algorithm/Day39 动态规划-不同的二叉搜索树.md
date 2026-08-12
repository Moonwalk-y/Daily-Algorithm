## 不同的二叉搜索树
### 链接
[96. 不同的二叉搜索树 - 力扣（LeetCode）](https://leetcode.cn/problems/unique-binary-search-trees/)
### 解法
解法其实很简单，有点像整数拆分，但是遍历部分脑筋需要转过来
dp数组定义是节点数为i的二叉搜索树的数量
初始化：i为0时`dp[i] = 1`，如果为0的话，左边无子树，最后乘积结果会是0
遍历：用i和j遍历。整数拆分中，j表示的是拆分出来的数，但是在这道题里，j要表示的是第j个节点作为根节点。否则边界条件等都不好判断
递推公式：`dp[i] = dp[j - 1] * dp[i - j]` 因为j是根节点，左子树就有j-1个节点，右子树有i-j个节点。
### 代码
```C++
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n + 1,0);
        dp[0] = 1;
        for(int i = 1;i <= n;i++){
            for(int j = 1;j <= i;j++){
                dp[i] += dp[j - 1] * dp[i - j];
            }
        }
        return dp[n];
    }
};
```