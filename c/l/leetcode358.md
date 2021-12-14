# Leetcode 358. Rearrange String k Distance Apart

[Explanation Video Link on Youtube](https://youtu.be/tBGXyjhM9FE)

[B站英文解说视频链接](https://www.bilibili.com/video/BV17L4y1H7km/)

```cpp
class Solution {
public:
    string rearrangeString(string s, int k) {
        if (k == 0) {
            return s;
        }
        unordered_map<char, int> c_to_count;
        
        for (const auto& c : s) {
            c_to_count[c]++;
        }
        
        priority_queue<pair<int, char>> pq;
        
        for (const auto& p : c_to_count) {
            pq.emplace(p.second, p.first);
        }
        
        string res = "";
        
        while (!pq.empty()) {
            vector<pair<int, char>> cache;
            auto [top_freq, top_c] = pq.top();
            
            if (top_freq > 1 && pq.size() < k) {
                return "";
            }
            
            for (int i = 0; i < k; i++) {
                if (pq.empty()) {
                    return res;
                }
                
                auto [freq, c] = pq.top();
                pq.pop();
                res += c;
                freq--;
                
                if (freq > 0) {
                    cache.emplace_back(freq, c);
                }
            }
            
            for (const auto element : cache) {
                pq.push(element);
            }
            
        }
        
        return res;
    }
};
```

