## 二叉搜索树的最小绝对差
[https://programmercarl.com/0530.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E5%B0%8F%E7%BB%9D%E5%AF%B9%E5%B7%AE.html#%E6%80%9D%E8%B7%AF]

---
#### 解法
利用二叉搜索树的性质，中序遍历得到递增序列，这样最小差值只有可能发生在序列相邻元素之间。然后最妙的地方就是用全局变量result、pre来记录目前最小值和上一个节点。这样就实现了一边中序遍历一边比较，不用把中序序列存下来再比较了，甚至这样`root->val - pre->val`都不用加绝对值，因为是递增序列，一定是正的，妙啊这个方法，然后每次运算前再判断一下pre是不是空，这样就可以处理最左下角第一个节点pre为空的边界情况。
```
class Solution {
private:
    TreeNode* pre;
    int result = INT_MAX;
public:
    void visit(TreeNode* root){
        if(!root) return;
        visit(root->left);
        if(pre){
            result = min(result,root->val - pre->val);
        }
        pre = root;
        visit(root->right);
    }
    int getMinimumDifference(TreeNode* root) {
        visit(root);
        return result;
    }
};
```

## 二叉搜索树中的众数
[https://programmercarl.com/0501.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E4%BC%97%E6%95%B0.html#%E6%80%9D%E8%B7%AF]

---
#### 解法
这个题也妙的不行，因为众数可能不止一个，先设一个maxcount，如果count等于maxcount就加到结果数组里，但如果发现有比maxcount更大的count，也不用管有没有统计完，直接更新maxcount，并且清空res数组重新push。
```
class Solution {
private:
    int max_count = 0;
    int count;
    TreeNode* pre;
    vector<int> res;
public:
    void visit(TreeNode* root){
        if(!root) return;
        visit(root->left);
        if(!pre) count = 1;
        else if(pre->val == root->val) count++;
        else count = 1;
        pre = root;
        if(count == max_count){
            res.push_back(root->val);
        }
        if(count > max_count){
            res.clear();
            res.push_back(root->val);
            max_count = count;
        }
        visit(root->right);
    }
    vector<int> findMode(TreeNode* root) {
        visit(root);
        return res;
    }
};
```

## 二叉树的最近公共祖先
[https://programmercarl.com/0236.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.html#%E7%AE%97%E6%B3%95%E5%85%AC%E5%BC%80%E8%AF%BE]

---
#### 解法
这题的解法更是妙到家了，本质上和用true、false判断这一路有没有目标节点是一样的，只不过这里返回的是`TreeNode*`，可以用来记录节点。如果发现了目标节点，那就一路回溯。两种情况：
1.两个目标节点位于两个树枝上，这样当前节点的left和right遍历结果都不空，这样就返回当前的节点即可
2.两个目标有一个是另一个的祖先，这样子公共祖先就是那个位置高的节点，这样就返回这个位置高的节点就可以。因为不会再有一个节点出现结果一的情况，所以这个节点会一路返回上去成为答案。
```
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == p || root == q || !root) return root;
        TreeNode* left = lowestCommonAncestor(root->left,p,q);
        TreeNode* right = lowestCommonAncestor(root->right,p,q);
        if(left && right) return root;
        else if(!left && right) return right;
        else if(left && !right) return left;
        else return nullptr;
    }
};
```
