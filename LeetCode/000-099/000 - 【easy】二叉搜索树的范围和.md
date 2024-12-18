原题：[938. 二叉搜索树的范围和](https://leetcode.cn/problems/range-sum-of-bst/) 

#### 题意：
给定二叉搜搜索树，让你对值在$[low,high]$之间的求和

#### 题解：
本题考察***二叉搜索树***的*性质*，只需要通过==**递归分治**==就可以解决。
二叉搜索树的性质：
	
	- 左子树中所有结点的值，均小于其根结点的值。
	- 右子树中所有结点的值，均大于其根结点的值。
	- 二叉搜索树的子树也是二叉搜索树。

#### 代码：
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int rangeSumBST(struct TreeNode* root, int low, int high){
    if(root == NULL) return 0;
    if(root->val > high)    //如果最大的值都比当前结点值小，那么肯定在左边才能找到
        return rangeSumBST(root->left, low, high);
    else if(root->val < low)   //如果最小值都比当前结点大，那么肯定在右边才能找到
        return rangeSumBST(root->right, low, high);
    else    //这种情况肯定是在范围内了，将当前结点值加上左右的，再返回
        return root->val + rangeSumBST(root->right, low, high) + rangeSumBST(root->left, low, high);
    }
};
```
