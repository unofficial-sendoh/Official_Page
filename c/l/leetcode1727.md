# Leetcode1727. Largest Submatrix With Rearrangements

[Explanation Video Link on Youtube](https://youtu.be/7hhhKEAOLEs)

[B站中文解说视频链接](https://www.bilibili.com/video/BV1CF41187Si)

```cpp
class Solution {
public:
    int largestSubmatrix(vector<vector<int>>& matrix) {
        int n = matrix.size();
        int m = matrix[0].size();
        vector<int> ones_above(m, 0);
        int max_area = 0;
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (matrix[i][j] == 1) {
                    ones_above[j]++;
                }
                else {
                    ones_above[j] = 0;
                }
            }
            
            vector<int> helper = ones_above;
            sort(helper.begin(), helper.end(), std::greater<int>());
            
            for (int j = 0; j < m; j++) {
                if (helper[j] > 0) {
                    max_area = max(max_area, (j + 1) * helper[j]);
                }
            }
        }
        
        return max_area;
    }
};
```
