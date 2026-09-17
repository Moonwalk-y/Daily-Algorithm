## 单词拆分
### 链接
[139. 单词拆分 - 力扣（LeetCode）](https://leetcode.cn/problems/word-break/)
### 解法
使用动态规划解决，将wordDict变成unordered_set，用于匹配是否出现过。
dp数组：`dp[i] = true`表示s的前i个字符组成的字符串可以使用字典里的词拼成
递推公式：`if(words.find(word) != words.end() && dp[j] == true)dp[i] = true;`
遍历顺序：先遍历背包，再遍历物品。也就是先遍历s，然后find相当于是在遍历物品，找能匹配的。
### 代码
```C++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> words(wordDict.begin(),wordDict.end());
        vector<bool> dp(s.size() + 1,false);
  
        dp[0] = true;
  
        for(int i = 1;i <= s.size();i++){
            for(int j = 0;j < i;j++){
                string word = s.substr(j,i-j);
                if(words.find(word) != words.end() && dp[j] == true){
                    dp[i] = true;
                }
            }
        }
        return dp[s.size()];
    }
};
```