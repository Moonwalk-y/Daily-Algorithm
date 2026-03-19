## 找树左下角的值
[https://programmercarl.com/0513.%E6%89%BE%E6%A0%91%E5%B7%A6%E4%B8%8B%E8%A7%92%E7%9A%84%E5%80%BC.html#%E6%80%9D%E8%B7%AF]

#### 解法
采用记录层次的层序遍历，最下面一层的第一个就是结果，通过if(i == 0) result = root->val取得。
```
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        queue<TreeNode*> que;
        int result = 0;
        que.push(root);
        while(!que.empty()){
            int size = que.size();
            for(int i = 0;i < size;i++){
                root = que.front();que.pop();
                if(i == 0) result = root->val;
                if(root->left) que.push(root->left);
                if(root->right) que.push(root->right);
            }
        }
        return result;
    }
};
```
---

## 路径总和
[https://programmercarl.com/0112.%E8%B7%AF%E5%BE%84%E6%80%BB%E5%92%8C.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

#### 解法
找到了符合条件的就返回true，重点是怎么一层层把这个true返回到顶层。
```
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        return finder(root,targetSum,0);
    }
    bool finder(TreeNode* root,int tar,int val){
        if(!root) return false;
        val += root->val;
        if(!root->left && !root->right){
            if(val == tar) return true;
            else return false;
        }
        return finder(root->right,tar,val) || finder(root->left,tar,val);
    }
};
```

---
## 从中序与后序遍历序列构造二叉树
[https://programmercarl.com/0106.%E4%BB%8E%E4%B8%AD%E5%BA%8F%E4%B8%8E%E5%90%8E%E5%BA%8F%E9%81%8D%E5%8E%86%E5%BA%8F%E5%88%97%E6%9E%84%E9%80%A0%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E6%80%9D%E8%B7%AF]

#### 解法
还有种方法，不是新构造数组然后递归的时候传新数组，是用下标来划分。
```
class Solution {
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if(inorder.size() == 0) return nullptr;
        return builder(inorder,postorder);
    }

    TreeNode* builder(vector<int>& inorder, vector<int>& postorder){
        if(postorder.size() == 0) return nullptr;
        TreeNode* root = new TreeNode(postorder[postorder.size() - 1]);
        int index = 0;
        for(;index < inorder.size();index++){
            if(inorder[index] == root->val) break;
        }
        if(postorder.size() == 1) return root;

        vector<int> leftinorder(inorder.begin(),inorder.begin() + index);
        vector<int> rightinorder(inorder.begin() + index + 1,inorder.end());

        postorder.resize(postorder.size() - 1);
        vector<int> leftpostorder(postorder.begin(),postorder.begin() + leftinorder.size());
        vector<int> rightpostorder(postorder.begin() + leftinorder.size(),postorder.end());

        root->left = builder(leftinorder,leftpostorder);
        root->right = builder(rightinorder,rightpostorder);

        return root;
    }
};
```
