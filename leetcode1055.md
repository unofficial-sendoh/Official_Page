# Leetcode 1055. Shortest Way to Format String

{% embed url="https://youtu.be/OSZovOH9w44" %}

{% embed url="https://www.bilibili.com/video/BV1Lf4y1A76Y/" %}

```cpp
class Solution {
public:
    int shortestWay(string source, string target) {
        unordered_map<char, vector<int>> char_to_index;
        
        for (int i = 0; i < source.size(); i++) {
            char_to_index[source[i]].push_back(i);
        }
        
        int target_len = target.size();
        int source_ptr = -1;
        int copy_needed = 1;
        int target_index = 0;
        
        while (target_index < target_len) {
            char cur_element = target[target_index];
            
            if (!char_to_index.count(cur_element)) {
                return -1;
            }
            
            const auto& search_range = char_to_index.at(cur_element);
            auto it = upper_bound(search_range.begin(), search_range.end(), source_ptr);
            
            if (it == search_range.end()) {
                copy_needed++;
                source_ptr = -1;
            }
            else {
                source_ptr = *it;
                target_index++;
            }
            
        }
        
        return copy_needed;
    }
};
```

