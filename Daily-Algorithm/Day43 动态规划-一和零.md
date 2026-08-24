## 一和零
### 链接
[474. 一和零 - 力扣（LeetCode）](https://leetcode.cn/problems/ones-and-zeroes/description/)
### 解法
dp数组定义：`dp[i][j]`是最多m个0和n个1的子集的最大长度。
初始化：都初始化为0即可
遍历顺序：外层遍历物品，内层遍历容量。该题中的物品是各个子集，容量有两个维度，0的数量和1的数量，因此内层需要双重循环。注意内层循环需要从大到小遍历
递推公式：`dp[i][j] = max(dp[i][j],dp[i - zeronum][j - onenum] + 1)`，意思是要么不取当前子集，要么取当前子集。
### 代码
```C++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> dp(m + 1,vector<int>(n + 1, 0));
        int zeronum = 0;
        int onenum = 0;
  
        for(int k = 0;k < strs.size();k++){
            zeronum = 0;onenum = 0;
            for(auto &p: strs[k]){
                if(p == '0') zeronum++;
                else onenum++;
            }
            for(int i = m;i >= zeronum;i--){
                for(int j = n;j >= onenum;j--){
                    dp[i][j] = max(dp[i][j], dp[i - zeronum][j - onenum] + 1);
                }
            }
        }
        return dp[m][n];
    }
};
```
### 注意
- 01背包问题外层遍历物品，内层遍历容量，且容量要从大到小遍历，一是为了避免重复加入背包，二是为了边界条件更清晰。
- 然后递推公式记得使用max