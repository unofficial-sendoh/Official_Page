# Leetcode 426. Convert Binary Search Tree to Sorted Doubly Linked list

[Explanation Video Link on Youtube](https://youtu.be/0v3HDQDROGc)

[B站中文解说视频链接](https://www.bilibili.com/video/BV1v44y1v7JQ)

```cpp
class Solution {
public:
    Node* treeToDoublyList(Node* root) {
        if (!root) return nullptr;
        helper(root);
        m_prev->right = m_head;
        m_head->left = m_prev;
        return m_head;
    }

private:
    void helper(Node* node) {
        if (!node) {
            return;
        }
        
        if (!m_head && !node->left) {
            m_head = node;
            m_prev = node;
        }
        
        Node* origin_left = node->left;
        Node* origin_right = node->right;
        helper(origin_left);
        m_prev->right = node;
        node->left = m_prev;
        m_prev = node;
        helper(origin_right);
    }
    
    Node* m_prev = nullptr;
    Node* m_head = nullptr;
};
```
