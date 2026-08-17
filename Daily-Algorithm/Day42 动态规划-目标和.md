## 目标和
### 链接
[494. 目标和 - 力扣（LeetCode）](https://leetcode.cn/problems/target-sum/description/)
### 解法
这道题有点难想，虽然做出来了，但我感觉我没有完全理解。
首先是数学上的方法，x为正数和，y为负数和，则有等式：
x + y = sum
x - y = target
得到x = (target + sum) / 2
则目的就变成了凑够x有几种方法。

dp数组定义：`dp[j]`表示凑够j的最大种方法
**状态转移方程**：`dp[j] = dp[j] + dp[j - nums[i]]`
- 含义：凑出 `j` 的方案数 = **不选当前数字**的方案数（`dp[j]`）+ **选当前数字**的方案数（`dp[j - nums[i]]`）。
初始化：`dp[0] = 1`，容量为0时有一种方法，什么都不选（如果为0那么整个数组结果都是0）
遍历顺序：外层正序遍历物品，内存倒序遍历容量。
### 代码
```C++
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int p;
        int sum = 0;
        for(int &i : nums) sum += i;
        p = (target + sum) / 2;
        if(p < 0) return 0;
        if((target + sum) % 2 == 1 || (target + sum) % 2 == -1) return 0;
  
        vector<int> dp(p + 1);
        dp[0] = 1;
  
        for(int i = 0;i < nums.size();i++){
            for(int j = p;j >= nums[i];j--){
                dp[j] += dp[j - nums[i]];
            }
        }
  
        return dp[p];
    }
};
```