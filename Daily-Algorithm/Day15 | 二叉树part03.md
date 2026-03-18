## 平衡二叉树
[https://programmercarl.com/0110.%E5%B9%B3%E8%A1%A1%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

---
#### 解法
使用了自顶向下递归，会重复调用find_deepth，时间复杂度较高。还有一种时间复杂度更低的自底向上递归。
```
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        bool res;
        if(!root) return true;
        if(find_deepth(root->right) - find_deepth(root->left) > 1 || find_deepth(root->right) - find_deepth(root->left) < -1) return false;
        return isBalanced(root->left) & isBalanced(root->right);
    }

    int find_deepth(TreeNode* root){
        if(!root) return 0;
        return max(find_deepth(root->left),find_deepth(root->right)) + 1;
    }
};
```
#### 自底向上递归
```
class Solution {
public:
    int height(TreeNode* root) {
        if (root == NULL) {
            return 0;
        }
        int leftHeight = height(root->left);
        int rightHeight = height(root->right);
        if (leftHeight == -1 || rightHeight == -1 || abs(leftHeight - rightHeight) > 1) {
            return -1;
        } else {
            return max(leftHeight, rightHeight) + 1;
        }
    }

    bool isBalanced(TreeNode* root) {
        return height(root) >= 0;
    }
};
```
## 二叉树的所有路径
[https://programmercarl.com/0257.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%89%80%E6%9C%89%E8%B7%AF%E5%BE%84.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

---
#### 解法
```
class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> res;
        string path = "";
        find_leaf(root,path,res);
        return res;
    }

    void find_leaf(TreeNode* root,string path,vector<string> &res){
        if (root->right || root->left){
            path += to_string(root->val);
            path += "->";
            if(root->right) find_leaf(root->right,path,res);
            if(root->left) find_leaf(root->left,path,res);
        }
        else{
            path += to_string(root->val);
            res.push_back(path);
            return;
        }
    }
};
```

## 左叶子之和
[https://programmercarl.com/0404.%E5%B7%A6%E5%8F%B6%E5%AD%90%E4%B9%8B%E5%92%8C.html]

---
#### 解法
```
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        if(!root) return 0;
        if(!root->right && !root->left) return 0;
        return find(root->right,0) + find(root->left,1);
    }

    int find(TreeNode* root,int sign){
        if(!root) return 0;
        if(!root->right && !root->left && sign == 1) return root->val;
        return find(root->left,1) + find(root->right,0);
    }
};
```

## 完全二叉树的节点个数
[https://programmercarl.com/0222.%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E8%8A%82%E7%82%B9%E4%B8%AA%E6%95%B0.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

---
#### 解法
迭代的层序遍历，记录节点个数
```
class Solution {
public:
    int countNodes(TreeNode* root) {
        if(!root) return 0;
        queue<TreeNode*> que;
        que.push(root);
        int count = 0;
        while(!que.empty()){
            root = que.front();
            que.pop();
            count++;
            if(root->right) que.push(root->right);
            if(root->left) que.push(root->left);
        }
        return count;
    }
};
```
