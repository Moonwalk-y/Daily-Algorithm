## 完全平方数
### 链接
[279. 完全平方数 - 力扣（LeetCode）](https://leetcode.cn/problems/perfect-squares/)
### 解法
dp数组定义：`dp[j]`表示和为j的完全平方数的最少数量
初始化：和为0 的有0个
递推公式：`dp[j] = min(dp[j], dp[j - i * i] + 1)`

### 代码
```C++
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n + 1,INT_MAX);
  
        dp[0] = 0;
  
        for(int i = 1;i*i <= n;i++){
            for(int j = 1;j <= n;j++)
                if(j >= i*i) dp[j] = min(dp[j],dp[j - i*i] + 1);
            }
        }
        return dp[n];
    }
};
```

---