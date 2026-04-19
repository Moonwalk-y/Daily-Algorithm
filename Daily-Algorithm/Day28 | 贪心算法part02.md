## 买卖股票的最佳时机 II
[https://programmercarl.com/0122.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BAII.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

#### 解法
```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int res = 0;
        int sub;
        for(int i = 1;i < prices.size();i++){
            sub = prices[i] - prices[i - 1];
            if(sub > 0) res+= sub;
        }
        return res;
    }
};
```
## 跳跃游戏
[https://programmercarl.com/0055.%E8%B7%B3%E8%B7%83%E6%B8%B8%E6%88%8F.html]

#### 解法
for循环的终止条件不一定是固定的，可以是动态变化的，比如这里，使用cover作为终止条件，而cover在变化，刚好符合“跳到最大可覆盖范围的语义”。
```
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int cover = 0;
        if(nums.size() == 1) return true;
        for(int i = 0;i <= cover;i++){
            cover = max(cover,i + nums[i]);
            if(cover >= nums.size() - 1) return true;
        }
        return false;
    }
};
```
## 跳跃游戏 Ⅱ
[https://programmercarl.com/0045.%E8%B7%B3%E8%B7%83%E6%B8%B8%E6%88%8FII.html]

#### 解法
用cur来指示当前的最远位置，next表示接下来最远能跳到的位置，当i == cur的时候，说明需要更新cur 了，也就是再跳一步，res++，cur更新成next，然后判断，如果cur大于序列范围了，说明就可以跳到终点了。贪心贪的是每次都跳最大的距离。
```
class Solution {
public:
    int jump(vector<int>& nums) {
        if (nums.size() == 1) return 0;
        int cur = 0;
        int res = 0;
        int next = 0;
        for(int i = 0;i < nums.size();i++){
            next = max(next,nums[i] + i);
            if(i == cur){
                res++;
                cur = next;
                if(cur >= nums.size() - 1) break;
            }
        }
        return res;
    }
};
```
## K次取反后最大化的数组和
[https://programmercarl.com/1005.K%E6%AC%A1%E5%8F%96%E5%8F%8D%E5%90%8E%E6%9C%80%E5%A4%A7%E5%8C%96%E7%9A%84%E6%95%B0%E7%BB%84%E5%92%8C.html]

#### 解法
先一个for循环把负的全变成正的，然后如果有剩余的次数，在把0反复变。如果没0，那就把绝对值最小的反复变
```
class Solution {
static bool cmp(int a, int b) {
    return abs(a) > abs(b);
}
public:
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        sort(nums.begin(),nums.end(),cmp);
        int i = 0;
        for(int j = 0;j < nums.size();j++){
            if(nums[j] < 0 && k > 0){
                nums[j] = -nums[j];
                k--;
            }
        }
        while(k > 0){
            if(nums[i] == 0) k--;
            else{
                nums[nums.size() - 1] = -nums[nums.size() - 1];
                k--;
            }
        }
        int res = 0;
        for(int &p:nums){
            res += p;
        }
        return res;
    }
};
```
