# Leetcode 710. Random Pick with Blacklist

[Explanation Video Link on Youtube](https://youtu.be/yuopy\_x2gk8)

[B站英文解说视频链接](https://www.bilibili.com/video/BV1H64y1Y765)

```cpp
class Solution {
public:
    Solution(int N, vector<int>& blacklist) {
        set<int> s;
        
        for (const auto& num:blacklist) s.insert(num);
        
        int index = N - 1;
        valid_num = N - s.size();
        
        for (const auto& num:s){
            if (num < valid_num){
                while (s.count(index)){
                    index--;
                }
                
                m[num] = index;
                index--;
            }
        }
    }
    
    int pick() {
        int res = rand() % valid_num;
        
        if (m.count(res)){
            return m[res];
        }
        
        return res;
    }
    
private:
    unordered_map<int, int> m;
    int valid_num = 0;
};
```
