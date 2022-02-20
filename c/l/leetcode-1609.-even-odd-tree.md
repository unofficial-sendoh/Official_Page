# Leetcode 1609. Even Odd Tree

[Explanation Video Link on Youtube](https://youtu.be/9RZfsIrF1TI)

[B站中文解说视频链接](https://www.bilibili.com/video/BV1hm4y1d7PV/)

```cpp
class Solution {
public:
    bool isEvenOddTree(TreeNode* root) {
        int is_even = 1;
        queue<TreeNode*> q;
        q.push(root);
        
        while (!q.empty()) {
            int num = q.size();
            int prev_val = is_even == 1 ? 0 : INT_MAX;
                
            while (num) {
                TreeNode* cur_node = q.front();
                q.pop();
                num--;
                
                if (is_even == 1) {
                    if (cur_node->val % 2 == 0 || cur_node->val <= prev_val) {
                        return false;
                    }
                }
                else {
                    if (cur_node->val %2 == 1 || cur_node->val >= prev_val) {
                        return false;
                    }
                }
                
                prev_val = cur_node->val;
                
                if (cur_node->left) {
                    q.push(cur_node->left);
                }
                
                if (cur_node->right) {
                    q.push(cur_node->right);
                }
                
            }
            
            is_even = -is_even;
        }
        
        return true;
    }
};
```
