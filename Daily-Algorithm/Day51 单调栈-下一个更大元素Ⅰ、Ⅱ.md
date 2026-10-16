## 下一个更大元素Ⅰ
### 链接
[496. 下一个更大元素 I - 力扣（LeetCode）](https://leetcode.cn/problems/next-greater-element-i/description/)
### 解法
需要把nums1中的元素对应到nums2中，然后在nums2中寻找下一个更大元素，但是ans数组需要以nums1的顺序。所以可以先在nums2中找到下一个更大元素的ans2，然后根据对应关系构建ans1.
### 代码
```C++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        vector<int> ans1(nums1.size());
        vector<int> ans2(nums2.size());
        stack<int> st;
        int j = 0;
        int top = 0;
  
        for(int j = 0;j < nums2.size();j++){
            if(!st.empty()){
                top = st.top();
                while(!st.empty() && nums2[top] < nums2[j]){
                    ans2[top] = nums2[j];
                    st.pop();
                    if(!st.empty()) top = st.top();
                }  
                st.push(j);
            }else st.push(j);
        }
        while(!st.empty()){
            ans2[st.top()] = -1;
            st.pop();
        }
  
        for(int i = 0;i < nums1.size();i++){
            j = 0;
            while(nums1[i] != nums2[j]){
                j++;
            }
            ans1[i] = ans2[j];
        }
  
        return ans1;
    }
};
```

## 下一个更大元素Ⅱ
### 链接
[503. 下一个更大元素 II - 力扣（LeetCode）](https://leetcode.cn/problems/next-greater-element-ii/description/)

### 解法
本题给的是一个循环数组来找下一个更大元素。可以将索引取模来实现循环的效果。
同时，其实不需要索引j，直接取top即可，代码会简洁许多。也不需要最后单独将栈内剩下的元素遍历取出并赋值-1，直接将ans设成全-1，最后没更新的自然还是-1.

### 代码
```C++
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        int size_ = nums.size();
        vector<int> ans(size_,-1);
        stack<int> st;
  
        for(int ind = 0;ind < 2 * size_;ind++){
            int i = ind % size_;
            if(!st.empty()){
                while(!st.empty() && nums[st.top()] < nums[i]){
                    ans[st.top()] = nums[i];
                    st.pop();
                }
                if(ind < size_) st.push(i);
            }else st.push(i);
        }
  
        return ans;
    }
};
```