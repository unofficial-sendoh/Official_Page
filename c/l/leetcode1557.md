# Leetcode 1557. Minimum Number of Vertices to Reach All Nodes

[Explanation Video Link on Youtube](https://youtu.be/BBsV1rXXWBc)

[B站英文解说视频链接](https://www.bilibili.com/video/BV1zL4y1t7bw?share\_source=copy\_web)

```cpp
class Solution {
public:
    vector<int> findSmallestSetOfVertices(int n, vector<vector<int>>& edges) {
        vector<int> node(n, 0);
        
        for (int i = 0; i < edges.size(); i++) {
            node[edges[i][1]]++;
        }
        
        vector<int> res;
        
        for (int i = 0;i < n; i++) {
            if (node[i] == 0) {
                res.push_back(i);
            }
        }
        
        return res;
    }
};
```
