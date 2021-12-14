# Leetcode 370. Range Addition

[Explanation Video Link on Youtube](https://youtu.be/L6Sg12REBOE)

[B站英文解说视频链接](https://www.bilibili.com/video/BV18Q4y1h7pf)

```cpp
class Solution {
public:
    vector<int> getModifiedArray(int length, vector<vector<int>>& updates) {
        unordered_map<int, int> m;
        
        for (int i = 0; i < updates.size(); i++){
            m[updates[i][0]] += updates[i][2];
            m[updates[i][1] + 1] -= updates[i][2];
        }
        
        int remain = 0;
        vector<int> res;
        
        for (int i = 0; i < length; i++){
            if (m.count(i)) remain += m[i];
            res.push_back(remain);
        }
        
        return res;
    }
};
```
