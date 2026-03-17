## 二叉树前序遍历
[https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%80%92%E5%BD%92%E9%81%8D%E5%8E%86.html#%E6%80%9D%E8%B7%AF]

---
#### 解法
前中后的递归法大同小异，只贴前序递归遍历的代码
```
class Solution {
public:
    void travel(TreeNode* root,vector<int> &res){
        if(root == nullptr) return;
        res.push_back(root->val);
        travel(root->left,res);
        travel(root->right,res);
    }
    
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        travel(root,res);
        return res;
    }
};
```

---
## 二叉树迭代遍历
[https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E8%BF%AD%E4%BB%A3%E9%81%8D%E5%8E%86.html#%E6%80%9D%E8%B7%AF]

---
#### 前序、后序
这两种写法很相似，只要倒一下就可以了，下面给出后序的代码
```
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        stack<TreeNode*> st;
        st.push(root);
        while(!st.empty()){
            root = st.top();st.pop();
            if(root == nullptr) continue;
            res.push_back(root->val);
            st.push(root->left);
            st.push(root->right);
        }
        reverse(res.begin(),res.end());
        return res;
    }
};
```

#### 中序
方法与前序和后序不同。
```
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> res;
        while(root != nullptr || !st.empty()){
            if(root != nullptr){
                st.push(root);
                root = root->left;
            }else{
                root = st.top();st.pop();
                res.push_back(root->val);
                root = root->right;
            }
        }
        return res;
    }
};
```

## 二叉树层序遍历
[https://programmercarl.com/0102.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

---
#### 解法
层序遍历并不难，难的是怎么分辨哪些元素是哪一层的。下面的代码使用了一个level_size来记录每一层的元素数量。使用两个循环，外层循环终止条件是队列为空，而内层循环是处理层序中每一层的元素。每次处理一层，然后把下一层放入队列，这样下一次while循环就用level_size记录que的size，得到这一层的数量，下一个for循环只处理这些元素。以此类推。
```
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res;
        if(!root) return res;
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty()){
            int level_size = que.size();
            vector<int> level;
            res.push_back(level);
            for(int i = 0;i < level_size;i++){
                root = que.front();que.pop();
                res.back().push_back(root->val);
                if(root->left) que.push(root->left);
                if(root->right) que.push(root->right);
            }
        }
        return res;
    }
};
```
