## 翻转二叉树
[https://programmercarl.com/0226.%E7%BF%BB%E8%BD%AC%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

---
#### 解法
递归法。或者我注意到这个翻转之后中序结果刚好反过来，不用递归的话可以先中序遍历，然后数组翻转再重新建树，不知道行不行的通。
```
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        work(root);
        return root;
    }
    void work(TreeNode* root){
        if(!root) return;
        TreeNode* temp = root->right;
        root->right = root->left;
        root->left = temp;
        work(root->left);
        work(root->right);
    }
};
```

## 对称二叉树
[https://programmercarl.com/0101.%E5%AF%B9%E7%A7%B0%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E6%80%9D%E8%B7%AF]

---
#### 解法
递归法，先列出比较条件，然后递归。每次写两行递归，一行递归外侧，一行递归内侧。
```
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(!root) return true;
        return compare(root->left,root->right);
    }
    bool compare(TreeNode* leftNode,TreeNode* rightNode){
        if(leftNode == nullptr && rightNode != nullptr) return false;
        else if(leftNode != nullptr && rightNode == nullptr) return false;
        else if(leftNode == nullptr && rightNode == nullptr) return true;
        else if(leftNode->val != rightNode->val) return false;
        else{
            bool l = compare(leftNode->left,rightNode->right);
            bool r = compare(leftNode->right,rightNode->left);
            return l && r;
        }
    }
};
```

## 二叉树的最大深度
[]

---
#### 解法
下面是我的方法，递归的记录每一个叶子节点的深度，然后找一个最大的返回。
```
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(!root) return 0;
        vector<int> res;
        work(root,1,res);
        int max = res[0];
        for(int &p : res){
            if(p > max) max = p;
        }
        return max;
    }
    void work(TreeNode* root,int deepth,vector<int> &res){
        if(!root->left && !root->right){
            res.push_back(deepth);
            return;
        }else if(root->left && !root->right){
            work(root->left,deepth + 1,res);
        }else if(root->right && !root->left){
            work(root->right,deepth+1,res);
        }else{
            work(root->left,deepth + 1,res);
            work(root->right,deepth + 1,res);
        }
    }
};
```
#### 标准题解
题解的解法太tm妙了，深度直接在return里加1就行了。先递到空然后return 0，之后每归一层就+1，挑出左边和右边最大的那个。
```
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == nullptr) return 0;
        return max(maxDepth(root->left), maxDepth(root->right)) + 1;
    }
};
```

## 二叉树的最小深度
[https://programmercarl.com/0111.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E6%B7%B1%E5%BA%A6.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

---
#### 解法
```
class Solution {
public:
    int minDepth(TreeNode* root) {
        if(!root) return 0;
        vector<int> res;
        work(root,1,res);
        int min = res[0];
        for(int &p : res){
            if(p < min) min = p;
        }
        return min;
    }
    void work(TreeNode* root,int deepth,vector<int> &res){
        if(!root->left && !root->right){
            res.push_back(deepth);
            return;
        }else if(root->left && !root->right){
            work(root->left,deepth + 1,res);
        }else if(root->right && !root->left){
            work(root->right,deepth+1,res);
        }else{
            work(root->left,deepth + 1,res);
            work(root->right,deepth + 1,res);
        }
    }
};
```
