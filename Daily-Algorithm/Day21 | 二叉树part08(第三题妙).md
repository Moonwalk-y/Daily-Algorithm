## 修剪二叉搜索树
[https://programmercarl.com/0669.%E4%BF%AE%E5%89%AA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

#### 解法
这道题的解法我觉得是有必要背过的，代码很简单，但是很难想。本质就是发现不合法的节点就去它可能合法的节点里找：如果遇到合法的了，就把合法的返回（判断合法节点，需要将这个节点作为子树的根节点再递归判断，该节点的子树全部合法，才算该节点合法）；遇到不合法的，继续向可能合法的方向找，直到遇到nullptr，返回nullptr，效果就是这些不合法节点都被删掉了。
```
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if(!root) return root;
        if(root->val < low) return trimBST(root->right,low,high);
        else if(root->val > high) return trimBST(root->left,low,high);
        else{
            root->left = trimBST(root->left,low,high);
            root->right = trimBST(root->right,low,high);
            return root;
        }
    }
};
```

---

## 将有序数组转换为二叉搜索树
[https://programmercarl.com/0108.%E5%B0%86%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E8%BD%AC%E6%8D%A2%E4%B8%BA%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91.html]

#### 解法
用二分查找，每次找出中间值做根节点就可以确保平衡。
```
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return build(nums,0,nums.size()-1);
    }
    TreeNode* build(vector<int>& nums,int i,int j){
        if(i > j) return nullptr;
        int mid = (i + j) / 2;
        TreeNode* root = new TreeNode(nums[mid]);
        root->left = build(nums,i,mid-1);
        root->right = build(nums,mid + 1,j);
        return root;
    }
};
```

---

## 把二叉搜索树转换为累加树
[https://programmercarl.com/0538.%E6%8A%8A%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E8%BD%AC%E6%8D%A2%E4%B8%BA%E7%B4%AF%E5%8A%A0%E6%A0%91.html]

#### 解法
这解法太吊了，不需要设置全局变量。还有种方法是设置全局变量，然后倒着中序遍历一遍。
```
class Solution {
public:
    TreeNode* convertBST(TreeNode* root) {
        visit(root,0);
        return root;
    }
    int visit(TreeNode* root,int parent_val){
        if(!root) return parent_val;
        root->val += visit(root->right,parent_val);
        return visit(root->left,root->val);
    }
};
//解法二，全局遍历+逆中序遍历
class Solution {
private:
    int sum = 0;
public:
    TreeNode* convertBST(TreeNode* root) {
        if(!root) return nullptr;
        convertBST(root->right);
        root->val += sum;
        sum = root->val;
        convertBST(root->left);
        return root;
    }
};
```

---
