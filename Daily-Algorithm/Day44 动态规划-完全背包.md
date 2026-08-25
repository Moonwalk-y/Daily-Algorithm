## 完全背包
### 链接
[完全背包理论基础-二维DP数组 | 完全背包 | 二维DP数组 | 状态转移 | 代码随想录-全网最全算法数据结构刷题学习路线|图文+视频教程|免费开源](https://programmercarl.com/algo/dynamic-programming/complete-knapsack-basics.html#%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85)
### 解法
完全背包和01背包的区别在于内层循环正序，因为这样可以重复取物品。
dp数组、递推公式等都和01背包一样
注意：下标都从1开始，避免混乱
### 代码
```C++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
int main() {
    int n, v;
    cin >> n >> v;
    
    vector<int> w(n + 1);
    vector<int> va(n + 1);
    
    // 从1开始存储，方便后续处理
    for (int i = 1; i <= n; i++) {
        cin >> w[i];
        cin >> va[i];
    }
    
    vector<vector<int>> dp(n + 1, vector<int>(v + 1, 0));
    
    // 完全背包（一维优化版更简洁）
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= v; j++) {
            if (j < w[i]) {
                dp[i][j] = dp[i-1][j];  // 装不下
            } else {
                dp[i][j] = max(dp[i-1][j], dp[i][j - w[i]] + va[i]);
                // 注意这里是 dp[i][j-w[i]]，表示可以重复取
            }
        }
    }
    
    cout << dp[n][v] << endl;
    return 0;
}
```