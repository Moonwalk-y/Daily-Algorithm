## 最大二叉树
[https://programmercarl.com/0654.%E6%9C%80%E5%A4%A7%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

---
#### 解法
注意边界条件，搞清楚是左闭右开还是左闭右闭。
```
class Solution {
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        return build(nums,0,nums.size() - 1);
    }

    TreeNode* build(vector<int> &nums,int start,int end){
        if(start > end) return nullptr;
        int max_index = start;
        if(start != end){
            for(int i = start;i <= end;i++){
                if(nums[i] > nums[max_index]) max_index = i;
            }
        }
        TreeNode* root = new TreeNode(nums[max_index]);
        root->left = build(nums,start,max_index - 1);
        root->right = build(nums,max_index + 1,end);
        return root;
    }
};
```

## 合并二叉树
[https://programmercarl.com/0617.%E5%90%88%E5%B9%B6%E4%BA%8C%E5%8F%89%E6%A0%91.html]

---
#### 解法
```
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if(!root1) return root2;
        if(!root2) return root1;
        root1->val += root2->val;
        root1->left = mergeTrees(root1->left,root2->left);
        root1->right = mergeTrees(root1->right,root2->right);
        return root1;
    }
};
```

## 二叉搜索树中的搜索
[https://programmercarl.com/0700.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E6%90%9C%E7%B4%A2.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

---
#### 解法
```
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        if(!root) return nullptr;
        if(root->val == val) return root;
        else if(root->val > val) return searchBST(root->left,val);
        else return searchBST(root->right,val);
    }
};
```

## 验证二叉搜索树
[https://programmercarl.com/0098.%E9%AA%8C%E8%AF%81%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

---
#### 解法
二叉搜索树的中序遍历得到的序列一定是递增的。
```
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        vector<int> tree;
        visit(root,tree);
        for(int i = 0;i < tree.size() - 1;i++){
            if(tree[i] >= tree[i + 1]) return false;
        }
        return true;
    }
    void visit(TreeNode* root,vector<int> &tree){
        if(!root) return;
        visit(root->left,tree);
        tree.push_back(root->val);
        visit(root->right,tree);
    }
};
```
