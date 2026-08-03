## 用栈实现队列
[https://programmercarl.com/0232.%E7%94%A8%E6%A0%88%E5%AE%9E%E7%8E%B0%E9%98%9F%E5%88%97.html]

---
#### 解法
```
class MyQueue {
private:
    stack<int> in;
    stack<int> out;
    void i2o(){
        while(!in.empty()){
            out.push(in.top());
            in.pop();
        }
    }
public:
    MyQueue() {
    }
    
    void push(int x) {
        in.push(x);
    }
    
    int pop() {
        if(out.empty()){
            i2o();
        }
        int num = out.top();
        out.pop();
        return num;
    }
    
    int peek() {
        if(out.empty()){
            i2o();
        }
        return out.top();
    }
    
    bool empty() {
        if(in.empty() && out.empty()) return true;
        return false;
    }
};
```

## 用队列实现栈
[https://programmercarl.com/0225.%E7%94%A8%E9%98%9F%E5%88%97%E5%AE%9E%E7%8E%B0%E6%A0%88.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

---
#### 解法
```
class MyStack {
private:
    queue<int> q;
public:
    MyStack() {
    }
    
    void push(int x) {
        q.push(x);
    }
    
    int pop() {
        int num = q.size();
        int n;int i = 0;
        while(i != num - 1){
            n = q.front();
            q.pop();
            q.push(n);
            i++;
        }
        n = q.front();q.pop();
        return n;
    }
    
    int top() {
        int num = q.size();
        int n;int i = 0;
        while(i < num - 1){
            n = q.front();
            q.pop();
            q.push(n);
            i++;
        }
        n = q.front();q.pop();q.push(n);
        return n;
    }
    
    bool empty() {
        if(q.empty()) return true;
        return false;
    }
};
```

## 有效的括号
[https://programmercarl.com/0020.%E6%9C%89%E6%95%88%E7%9A%84%E6%8B%AC%E5%8F%B7.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

#### 解法
```
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;
        for(auto &p : s){
            if(is_left(p)){
                st.push(p);
            }else{
                if(st.empty() || !is_match(p,st.top())){
                    return false;
                }else{
                    st.pop();
                }
            }
        }
        if(st.empty()){
            return true;
        }else{
            return false;
        }
    }

    bool is_left(char p){
        if(p == '(' || p == '[' || p == '{'){
            return true;
        }else{
            return false;
        }
    }

    bool is_match(char p,char t){
        if(t == '(' && p == ')'){
            return true;
        }else if(t == '[' && p == ']'){
            return true;
        }else if(t == '{' && p == '}'){
            return true;
        }else{
            return false;
        }
    }
};

```
## 删除字符串中的所有相邻重复项
[https://programmercarl.com/1047.%E5%88%A0%E9%99%A4%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%AD%E7%9A%84%E6%89%80%E6%9C%89%E7%9B%B8%E9%82%BB%E9%87%8D%E5%A4%8D%E9%A1%B9.html]

---
#### 解法
```
class Solution {
public:
    string removeDuplicates(string s) {
        stack<char> st;
        for(char &p : s){
            if(st.empty()) st.push(p);
            else{
                if(p == st.top()){
                    st.pop();
                }else{
                    st.push(p);
                }
            }
        }
        string res;
        while(!st.empty()){
            res.push_back(st.top());
            st.pop();
        }
        reverse(res.begin(),res.end());
        return res;
    }
};
```
