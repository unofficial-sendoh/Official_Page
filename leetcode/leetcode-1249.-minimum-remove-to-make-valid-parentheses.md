# Leetcode 1249. Minimum Remove to Make Valid Parentheses

{% embed url="https://youtu.be/N6At0J9UKS8" %}

{% embed url="https://www.bilibili.com/video/BV1Qb4y117yf/" %}



```cpp
class Solution {
public:
    string minRemoveToMakeValid(string s) {
        stack<int> left_parentheses;
        unordered_set<int> to_delete_parentheses;
        int str_size = s.size();

        for (int i = 0; i < str_size; i++) {
            if (s[i] == '(') {
                left_parentheses.push(i);
            }
            else if (s[i] == ')') {
                if (left_parentheses.empty()) {
                    to_delete_parentheses.insert(i);
                }
                else {
                    left_parentheses.pop();
                }
            }
        }

        while (!left_parentheses.empty()) {
            const auto top_index = left_parentheses.top();
            left_parentheses.pop();
            to_delete_parentheses.insert(top_index);
        }

        string res_str = "";

        for (int i = 0; i < str_size; i++) {
            if (!to_delete_parentheses.count(i)) {
                res_str += s[i];
            }
        }

        return res_str;

    }
};
```

