# Leetcode 971. Flip Binary Tree To Match Preorder Traversal

{% embed url="https://youtu.be/EMsPe9YN1v8" %}

```cpp
class Solution {
public:
    vector<int> flipMatchVoyage(TreeNode* root, vector<int>& voyage) {
        if (!helper(root, voyage)) {
            return {-1};
        }
        
        return m_res;
    }
    
private:
    bool helper(TreeNode* node, const vector<int>& voyage) {
        if (!node) {
            return true;
        }
        
        if (voyage[m_cur_index] != node->val) {
            return false;
        }
        
        m_cur_index++;
        bool is_left_valid = false;
        bool is_right_valid = false;
        
        if (node->left && node->left->val != voyage[m_cur_index]) {
            m_res.push_back(node->val);
            is_left_valid = helper(node->right, voyage);
            is_right_valid = helper(node->left, voyage);
        }
        else {
            is_left_valid = helper(node->left, voyage);
            is_right_valid = helper(node->right, voyage);
        }
        
        return is_left_valid && is_right_valid;
    }
    
    vector<int> m_res;
    int m_cur_index = 0;
};
```

