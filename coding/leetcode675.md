# Leetcode 675. Cut Off Trees for Golf Event

{% embed url="https://youtu.be/1hYC9ySnJhA" %}

```cpp
class Solution {
public:
    int cutOffTree(vector<vector<int>>& forest) {
        vector<tuple<int, int, int>> cell_metadata;
        m_m = forest.size();
        m_n = forest[0].size();
        
        for (int i = 0; i < m_m; i++) {
            for (int j = 0; j < m_n; j++) {
                if (forest[i][j] > 1) {
                    cell_metadata.emplace_back(forest[i][j], i, j);
                }
            }
        }
        
        sort(cell_metadata.begin(), cell_metadata.end());
        int cur_x = 0;
        int cur_y = 0;
        int total_step = 0;
        
        for (int i = 0; i < cell_metadata.size(); i++) {
            const auto& [_, target_x, target_y] = cell_metadata[i];
            int step = bfs(cur_x, cur_y, target_x, target_y, forest);
            
            if (step < 0) {
                return -1;
            }

            total_step += step;
            forest[target_x][target_y] = 1;
            cur_x = target_x;
            cur_y = target_y;
        }
        
        return total_step;
        
    }
    
private:
    int m_m;
    int m_n;
    
    int bfs(int cur_x, int cur_y, int target_x, int target_y, const vector<vector<int>>& forest) {
        int step = 0;
        queue<pair<int, int>> q;
        q.emplace(cur_x, cur_y);
        vector<int> dirs = {0, -1, 0, 1, 0};
        auto visited = vector<vector<int>>(m_m, vector<int>(m_n, 0));
        visited[cur_x][cur_y] = 1;
        
        while (!q.empty()) {
            int n = q.size();
            
            while (n) {
                const auto [x, y] = q.front();
                q.pop();
                n--;
                
                if (x == target_x && y == target_y) {
                    return step;
                }
                
                for (int i = 0; i < 4; i++) {
                    int new_x = x + dirs[i];
                    int new_y = y + dirs[i + 1];
                    
                    if (new_x < 0 || new_y < 0 || new_x == m_m || new_y == m_n || visited[new_x][new_y] || forest[new_x][new_y] == 0) {
                        continue;
                    }
                    
                    q.emplace(new_x, new_y);
                    visited[new_x][new_y] = 1;
                }
                

            }
            
            step++;
        }
        
        return -1;
    }
};
```

