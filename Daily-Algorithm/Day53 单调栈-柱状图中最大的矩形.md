### 链接
[84. 柱状图中最大的矩形 - 力扣（LeetCode）](https://leetcode.cn/problems/largest-rectangle-in-histogram/description/)

### 解法
要求柱状图中的最大矩形，也就是要得到高度和宽度最大的矩形。方法就是找到每个柱子的左边和右边更低的那个柱子，两个低柱子中间就是宽度，然后这个柱子自己的高就是局部最大高度。
而要求 下一个更大/更小就应该使用单调栈解决。
### 代码
```C++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> st;
        int n = heights.size();
        vector<int> left(n,-1), right(n,n);
        int ans = 0;
  
        for(int i = 0;i < n;i++){
            while(!st.empty() && heights[i] < heights[st.top()]){
                right[st.top()] = i;
                st.pop();
            }
            st.push(i);
        }
        while(!st.empty()){st.pop();}
        for(int i = n-1;i >= 0;i--){
            while(!st.empty() && heights[i] < heights[st.top()]){
                left[st.top()] = i;
                st.pop();
            }
            st.push(i);
        }
        for(int i = 0;i < n;i++){
            ans = max(ans, (right[i] - left[i] - 1) * heights[i]);
        }
        return ans;
    }
};
```