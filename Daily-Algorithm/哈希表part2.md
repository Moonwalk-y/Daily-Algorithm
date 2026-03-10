## 四数相加Ⅱ
[https://programmercarl.com/0454.%E5%9B%9B%E6%95%B0%E7%9B%B8%E5%8A%A0II.html#%E6%80%9D%E8%B7%AF]

#### 思路
使用unordered_map，因为需要记录出现的次数。然后两两一组寻找符合要求的匹配，重点是掌握unordered_map的用法
```
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int,int> map;
        for(int &i : nums1){
            for(int &j : nums2){
                map[i + j]++;
            }
        } 
        int count = 0;
        for(int &i : nums3){
            for(int &j : nums4){
                if(map.find(-i - j) != map.end()){
                    count += map[-i - j];
                }
            }
        }
        return count;
    }
};
```
#### 总结
- 初始化：`unordered_map<int,int> a`;
- 访问：`a[i]`
- 查找：类似于unordered_set

---
## 赎金信
[https://programmercarl.com/0383.%E8%B5%8E%E9%87%91%E4%BF%A1.html#%E6%80%9D%E8%B7%AF]

---
#### 解法
依旧是使用unordered_map，比较常规，和前面的字母异位词差不多。
```
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<char,int> usable;
        for(char &p : magazine){
            usable[p]++;
        }
        for(char &p : ransomNote){
            usable[p]--;
            if(usable[p] < 0) return false;
        }
        return true;
    }
};
```

---
## 三数之和
[https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html#%E6%80%9D%E8%B7%AF]

---
#### 解法
本题使用双指针解决（三指针），因为要去重，所以不方便使用哈希表。
先对数组进行排序，然后用一个i遍历数组，初始为i = 0，如果`nums[i]`在排序后大于0，则直接break。然后设置`left = i + 1`，`right = nums.size() - 1`，当三个数加起来大于0时right--；小于0时left++；等于0时将三个数放入结果数组
关于去重：
- i的去重。当`nums[i] == nums[i - 1]`时continue，因为如果是i == i + 1时continue，语义是结果组中不能出现相同元素，像（-1，-1，2）就漏掉了。所以应该是i == i - 1，这样才能跳过已经使用了的元素。
- left、right的去重。这里不用和上一个比，和下一个比就可以。因为在比较前已经将结果加进结果数组了，之后的比较都是在已经存好结果的条件下比，直接看下一个和已经存进去的一不一样就可以了。
```
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(),nums.end());
        for(int i = 0;i < nums.size() - 2;i++){
            if(nums[i] > 0) break;
            if(i > 0 && nums[i] == nums[i - 1]) continue;
            int left = i + 1;
            int right = nums.size() - 1;

            while(left < right){
                if(nums[i] + nums[left] + nums[right] < 0) left++;
                else if(nums[i] + nums[left] + nums[right] > 0) right--;
                else{
                    result.push_back({nums[i],nums[left],nums[right]});
                    while(left < right && nums[left] == nums[left + 1]) left ++;
                    while(left < right && nums[right] == nums[right - 1]) right--;
                    right--;left++;
                }
            }
        }
        return result;
    }
};
```
#### 总结
right和left的去重要放在else里，不能提前拿出来。

---
## 四数之和
[https://programmercarl.com/0018.%E5%9B%9B%E6%95%B0%E4%B9%8B%E5%92%8C.html#%E6%80%9D%E8%B7%AF]

---
#### 解法
对j的去重，要放在后面，和left、right的去重逻辑类似，不可以采用和i一样的，因为当`nums[i] == nums[j - 1]`的时候会直接跳过
```
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(),nums.end());
        if(nums.size() < 4) return result;
        for(int i = 0;i < nums.size() - 3;i++){
            for(int j = i + 1;j < nums.size() - 2;j++){
                if(nums[i] > target && nums[i] > 0) break;
                if(i > 0 && nums[i] == nums[i - 1]) continue;
                int left = j + 1;
                int right = nums.size() - 1;

                while(left < right){
                    if((long)nums[i] + nums[j] + nums[left] + nums[right] < target) left++;
                    else if((long)nums[i] + nums[j] + nums[left] + nums[right] > target) right--;
                    else{
                        result.push_back({nums[i],nums[j],nums[left],nums[right]});
                        while(left < right && nums[left] == nums[left + 1]) left ++;
                        while(left < right && nums[right] == nums[right - 1]) right--;
                        while(j < nums.size() - 2 && nums[j] == nums[j + 1]) j++;
                        right--;left++;
                    }
                }
            }
        }
        return result;
    }
};
```
