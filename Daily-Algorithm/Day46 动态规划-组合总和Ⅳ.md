## 组合总和Ⅳ
### 链接
[377. 组合总和 Ⅳ - 力扣（LeetCode）](https://leetcode.cn/problems/combination-sum-iv/)
### 解法
完全背包，但是有坑，因为说是求 组合总和，其实是要求排列总和，顺序不一样的也算，所以使用一维数组来解决。
dp数组：`dp[i]`的意思是凑成金额i的排列个数
初始化：凑成0的个数是1，就是什么都不选
递推公式：`dp[i] += dp[i - nums[j]]`
遍历顺序：
**如果求组合数就是外层for循环遍历物品，内层for遍历背包**。
**如果求排列数就是外层for遍历背包，内层for循环遍历物品**。
如果把遍历nums（物品）放在外循环，遍历target的作为内循环的话，举一个例子：计算dp[4]的时候，结果集只有 {1,3} 这样的集合，不会有{3,1}这样的集合，因为nums遍历放在外层，3只能出现在1后面！
### 代码
```C++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<unsigned long long> dp(target+1);
        dp[0] = 1;
        for(int i = 1;i <= target;i++){
            for(int j = 0;j < nums.size();j++){
                if(i >= nums[j]) dp[i] += dp[i - nums[j]];
            }
        }
        return dp[target];
    }
};
```