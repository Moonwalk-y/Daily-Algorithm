### 链接
[739. 每日温度 - 力扣（LeetCode）](https://leetcode.cn/problems/daily-temperatures/description/)

### 解法
题目要求返回第i天下一个更高温度的天之间有几天，使用单调栈解决，栈中存的是数组下标，也就是天数。递减栈，如果发现更大的元素，就弹出栈顶元素，更大元素下标减掉弹出元素（下标）就是要求的天数，循环，直到栈空或者栈顶元素大于当前元素，当前元素入栈。
最后栈中剩下的就是没有更大元素的天，把他们赋值成0即可。

### 代码
```C++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<int> wait;
        vector<int> answer(temperatures.size());
        int curtop = 0;
        for(int i = 0;i < temperatures.size();i++){
            if(!wait.empty()){
                curtop = wait.top();
                while(!wait.empty() && temperatures[curtop] < temperatures[i]){
                    answer[curtop] = i - curtop;
                    wait.pop();
                    if(!wait.empty())curtop = wait.top();
                }
                wait.push(i);
            }else wait.push(i);
        }
        while(!wait.empty()){
            answer[wait.top()] = 0;
            wait.pop();
        }
        return answer;
    }
};
```