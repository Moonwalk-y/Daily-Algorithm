## 监控二叉树
### 链接：[968. 监控二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-cameras/description/)
### 解法

### 代码
```C++
class Solution {
public:
    int result = 0;
    int travel(TreeNode* root){
        if(root == nullptr) return 2;
        int left = travel(root->left);
        int right = travel(root->right);
  
        if(left == 2 && right == 2) return 0;
        if(left == 0 || right == 0){
            result++;
            return 1;
        }
        if(left == 1 || right == 1) return 2;
        return -1;
    }
    int minCameraCover(TreeNode* root) {
        if(travel(root) == 0){
            result++;
        }
        return result;
    }
};
```

---