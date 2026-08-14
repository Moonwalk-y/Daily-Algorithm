## 分割等和子集
### 链接
[416. 分割等和子集 - 力扣（LeetCode）](https://leetcode.cn/problems/partition-equal-subset-sum/description/)
### 解法
核心想法是先算出sum，然后凑sum/2，如果sum是奇数，那肯定凑不出来，直接返回false
dp数组定义：如果是二维数组，`dp[i][j]`含义是前i个数字在容量为j的情况下凑出的最大值
初始化：采用一维数组方式解决，全初始化为0即可。
遍历顺序：i从前到后，j从后往前，为了避免重复添加物品。j要大于等于`nums[i]`，因为背包容量必须足够才可以
递推公式：`dp[j] = max(dp[j],dp[j - nums[i]] + nums[i])`
最后判断`dp[target]`是否等于target。
### 代码
```C++
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum = 0;
        for(int &i:nums) sum += i;
        if(sum % 2 == 1) return false;
        int target = sum / 2;
  
        vector<int> dp(target + 1,0);
  
        for(int i = 0;i < nums.size();i++){
            for(int j = target;j >= nums[i];j--){
                dp[j] = max(dp[j],dp[j-nums[i]] + nums[i]);
            }
        }
  
        if(dp[target] == target) return true;
        else return false;
    }
};
```

---
## 最后一块石头的重量
### 链接
[1049. 最后一块石头的重量 II - 力扣（LeetCode）](https://leetcode.cn/problems/last-stone-weight-ii/description/)
### 解法
和上一题很像，也是凑一半。
### 代码
```C++
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int sum = 0;
        for(int &i: stones) sum += i;
        int n = stones.size();
  
        vector<int> dp(sum / 2 + 1,0);
        for(int i = 0;i < n;i++){
            for(int j = sum/2;j >= stones[i];j--){
                dp[j] = max(dp[j],dp[j - stones[i]] + stones[i]);
            }
        }
  
        int res = abs(sum - dp[sum/2] * 2);
        return res;
    }
};
```