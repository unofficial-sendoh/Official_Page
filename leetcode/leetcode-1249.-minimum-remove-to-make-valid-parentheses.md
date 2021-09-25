# Leetcode 1249. Minimum Remove to Make Valid Parentheses

<iframe width="560" height="315" src="https://www.youtube.com/embed/N6At0J9UKS8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
{% highlight cpp %}
<iframe src="//player.bilibili.com/player.html?aid=632825380&bvid=BV1Qb4y117yf&cid=401761966&page=1&danmaku=0" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"   style="width: 640px; height: 430px; max-width: 100%"> </iframe>

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

