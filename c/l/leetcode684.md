# Leetcode 684. Redundant Connection

[Explanation Video Link on Youtube](https://youtu.be/puOZl3NokDc)

[B站英文解说视频链接](https://www.bilibili.com/video/BV1JY411x7Vv)

```cpp
class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        vector<int> pa(n + 1, 0);
        
        function<int(int)> find_parents = [&](int node) {
            if (pa[node] != node) {
                pa[node] = find_parents(pa[node]);
            }
            
            return pa[node];
        };
        
        for (const auto& edge : edges) {
            int u = edge[0];
            int v = edge[1];
            
            
            
            if (pa[u] == 0) {
                pa[u] = u;
            }
            
            if (pa[v] == 0) {
                pa[v] = v;
            }
            
            int pa_u = find_parents(u);
            int pa_v = find_parents(v);
            
            if (pa_u == pa_v) {
                return edge;
            }
            
            pa[pa_u] = pa_v;
        }
        
        return {0, 0};
    }
};
```
