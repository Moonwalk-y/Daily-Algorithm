## 二叉搜索树的最近公共祖先
[https://programmercarl.com/0235.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

#### 解法
没用二叉搜索树的性质，和之前普通二叉树的解法一样，就当复习了，挺妙的方法
```
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(!root || root == p || root == q) return root;
        TreeNode* left = lowestCommonAncestor(root->left,p,q);
        TreeNode* right = lowestCommonAncestor(root->right,p,q);
        if(left && right) return root;
        else if(left && !right) return left;
        else if(!left && right) return right;
        else return nullptr;
    }
};
```

---
## 二叉搜索树中的插入操作
[https://programmercarl.com/0701.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%8F%92%E5%85%A5%E6%93%8D%E4%BD%9C.html]

#### 解法
顺着往下找，找到空的直接插入就行，不会出现需要插入的地方有别的节点的。
```
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if(!root){
            return new TreeNode(val);
        }
        if(root->val > val){
            root->left = insertIntoBST(root->left,val);
        }else{
            root->right = insertIntoBST(root->right,val);
        }
        return root;
    }
};
```

---
## 删除二叉搜索树中的节点
[https://programmercarl.com/0450.%E5%88%A0%E9%99%A4%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.html]

#### 解法
```
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if(!root) return root;
        if(root->val > key){
            root->left = deleteNode(root->left,key);
            return root;
        }
        else if(root->val < key){
            root->right = deleteNode(root->right,key);
            return root;
        }
        else{
            if(!root->right && !root->left) return nullptr;
            else if(!root->right) return root->left;
            else if(!root->left) return root->right;
            else{
                TreeNode* target = root->right;
                while(target->left){
                    target = target->left;
                }
                root->right = deleteNode(root->right,target->val);
                target->right = root->right;
                target->left = root->left;
                return target;
            }
        }
    }
};
```
