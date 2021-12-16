# Leetcode 1110. Delete Nodes And Return Forest

[Explanation Video Link on Youtube](https://youtu.be/Kn9XwTdGTdQ)

[B站中文解说视频链接](https://www.bilibili.com/video/BV1VL4y1B7pF)

```cpp
class Solution {
public:
    vector<TreeNode*> delNodes(TreeNode* root, vector<int>& to_delete) {
        unordered_set<int> delete_set(to_delete.begin(), to_delete.end());
        
        function<vector<TreeNode*>(TreeNode*)> helper = [&](TreeNode* node)->vector<TreeNode*> {
            if (!node) {
                return {};
            }
            
            auto left_forest = helper(node->left);
            auto right_forest = helper(node->right);
            
            vector<TreeNode*> merged_forest;
            
            if (node->left) {
                if (delete_set.count(node -> val) && !delete_set.count(node -> left -> val)) {
                    merged_forest.push_back(node -> left);
                }
                
                if (delete_set.count(node->left->val)){
                    node->left = nullptr;
                }
                
            }
            
            if (node -> right) {
                if (delete_set.count(node -> val)  && !delete_set.count(node -> right -> val)) {
                    merged_forest.push_back(node -> right);
                }
                
                if (delete_set.count(node->right->val)) {
                    node->right = nullptr;
                }
            }
            
            for (const auto& elem : left_forest) {
                merged_forest.push_back(elem);
            }
            
            for (const auto& elem : right_forest) {
                merged_forest.push_back(elem);
            }
            
            return merged_forest;
            
        };
        
        auto res = helper(root);
        
        if (!delete_set.count(root->val)) {
            res.push_back(root);
        }
        return res;
    }
};
```
