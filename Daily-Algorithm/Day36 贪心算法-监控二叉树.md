## 监控二叉树
### 链接：[968. 监控二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-cameras/description/)
### 解法
要使用最少的监控，采用自下而上的贪心法。因为从上往下贪，头节点只有一个，但叶子节点有很多个，能节省更多的监控。

采用状态转移法，0表示没被覆盖，1表示有监控，2表示被覆盖

这样从叶子节点开始，如果遇到空，则返回空为2，被覆盖，因为如果为1，那么叶子节点是被覆盖，叶子节点的父节点就会是0，爷爷节点会有个监控，但实际上叶子节点是没被覆盖的；如果是0，那么叶子节点就会有一个监控，就不符合监控最少了。

这样当左右节点都是被覆盖，当前节点返回0；
左右节点有一个没覆盖，返回1，并且加一个监控；
左右节点有一个是监控，当前节点返回2。

注意，第二个判断必须在第三个之前，不然会出现左孩子为0，右孩子为1，然后节点不加监控的情况。

为了从下到上遍历，使用后序遍历。
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