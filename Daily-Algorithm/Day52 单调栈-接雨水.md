### 链接
[42. 接雨水 - 力扣（LeetCode）](https://leetcode.cn/problems/trapping-rain-water/description/)

### 解法
用一个单调递减的栈来存下标。当遇到一个更高的柱子时，如果栈里还有两个元素，最上面那个就是i，下面一个是left。因为是递减栈，left一定高于i，这样就形成了一个水坑，水坑宽度就是i - left - 1，高度是left和i里更小的那个。相乘就是子答案，ans +=即可。
可以理解为从下到上一层层的装雨水，一层装一个长方形。

### 代码
```C++
class Solution {
public:
    int trap(vector<int>& height) {
        int ans = 0;
        stack<int> st;
        int n = height.size();
  
        for(int i = 0;i < n;i++){
            while(!st.empty() && height[i] > height[st.top()]){
                int top = st.top();
                st.pop();
                if(st.empty()) break;
                int left = st.top();
                int wid = i - left - 1;
                int hgt = min(height[i],height[left]) - height[top];
                ans += wid * hgt;
            }
            st.push(i);
        }
  
        return ans;
    }
};
```