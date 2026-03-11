## 反转字符串
[https://programmercarl.com/0344.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2.html]

#### 解法
很常规，直接贴代码
```
class Solution {
public:
    void reverseString(vector<char>& s) {
        int i = 0;int j = s.size() - 1;
        char temp;
        while(i < j){
            temp = s[i];
            s[i] = s[j];
            s[j] = temp;
            i++;j--;
        }
        
    }
};
```

---
## 反转字符串Ⅱ
[https://programmercarl.com/0541.%E5%8F%8D%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2II.html#%E6%80%9D%E8%B7%AF]

#### 解法
用的文档里的解法，很巧妙，代码很简洁。不需要i++，直接i += 2k就可以了，然后每次循环判断i + k是否超出边界，不用管i + k后面的内容。
```
class Solution {
public:
    string reverseStr(string s, int k) {
        for(int i = 0;i < s.size();i += 2 * k){
            if(i + k <= s.size()){
                reverse(s.begin() + i,s.begin() + i + k);
            }else{
                reverse(s.begin() + i,s.end());
            }
        }
        return s;
    }
};
```

---
## 替换数字
[]

#### 解法
```
#include<iostream>
#include<vector>
#include<cctype>
#include<string>
using namespace std;

int main(){
    vector<string> s;
    char c;
    while(cin >> c){
        if(isdigit(c)){
            s.push_back("number");
        }else{s.push_back(string(1,c));}
    }
    for(auto &p : s){
        cout << p;
    }
}
```
#### 总结
记住cctype库以及里面的isdigit函数，还有就是string的类型转换：string(lenth,a)
