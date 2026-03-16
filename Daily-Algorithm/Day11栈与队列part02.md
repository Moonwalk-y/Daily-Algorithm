## 逆波兰表达式求值
[https://programmercarl.com/0150.%E9%80%86%E6%B3%A2%E5%85%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F%E6%B1%82%E5%80%BC.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

---
#### 解法
```
class Solution {
public:
    int calculate(int r,int l,string p){
        if(p == "+") return r + l;
        if(p == "-") return l - r;
        if(p == "*") return l * r;
        else return l / r;
    }

    int evalRPN(vector<string>& tokens) {
        stack<int> nums;
        for(auto &p : tokens){
            if(p != "+" && p != "-" && p != "*" && p != "/") nums.push(stoi(p));
            else{
                int r = nums.top();nums.pop();
                int l = nums.top();nums.pop();
                nums.push(calculate(r,l,p));
            }
        }
        return nums.top();
    }
};
```

## 滑动窗口最大值
[https://programmercarl.com/0239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.html#%E6%80%9D%E8%B7%AF]

---
#### 解法
采用一个单调队列deque来解决。在使用时对deque定义为：队首存当前窗口内的最大元素，每次向队尾push元素时，如果队尾元素小于要push的val，那么说明当前队尾元素不具备成为最大元素的条件，直接pop。当元素滑出窗口后，pop这个元素（如果该元素在队头）。那么为什么这个元素不会在队中呢？因为如果这个元素不在队首说明这个元素小于队首，一定是被pop掉了。细讲的话就是两种情况：
- 1.该元素>下一个元素，那么该元素就在队头，除非下下个元素更大，它也就被pop了。
- 2.该元素<下一个元素，那么该元素在下一个元素push的时候就pop了。
因此，可以看到pop时这个元素要么在队头，要么早就被pop了。
然后pop和push定义好之后就可以进行：pop，push，取值的循环了。
```
class Solution {
private:
    deque<int> que;
    void pop(int val){
        if(!que.empty() && val == que.front()) que.pop_front();
    }
    void push(int val){
        while(!que.empty() && que.back() < val) que.pop_back();
        que.push_back(val);
    }
    int front(){
        return que.front();
    }
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> result;
        for(int i = 0;i < k;i++){
            push(nums[i]);
        }
        result.push_back(que.front());
        for(int i = k;i < nums.size();i++){
            pop(nums[i - k]);
            push(nums[i]);
            result.push_back(que.front());
        }
        return result;
    }
};
```

## 前k个高频元素
[https://programmercarl.com/0347.%E5%89%8DK%E4%B8%AA%E9%AB%98%E9%A2%91%E5%85%83%E7%B4%A0.html#%E6%80%9D%E8%B7%AF]

---
#### 解法
使用unordered_map记录元素出现的次数，然后使用一个小顶堆进行比较。注意：
- 小顶堆的初始化：priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>> minHeap;
- 往小顶堆push的操作：minHeap.push({it.second,it.first});之所以second在前，是因为小顶堆默认比较的是二元组的第一个元素，所以我们把second也就是元素的出现次数放在前面做比较
```
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int,int> map;
        for(int i = 0;i < nums.size();i++){
            map[nums[i]]++;
        }

        priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>> minHeap;
        for(auto &it : map){
            minHeap.push({it.second,it.first});
            if(minHeap.size() > k){
                minHeap.pop();
            }
        }
        vector<int> res;
        for(int i = 0;i < k;i++){
            res.push_back(minHeap.top().second);
            minHeap.pop();
        }
        return res;
    }
};
```
