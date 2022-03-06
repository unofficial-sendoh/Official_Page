# Closest Coin

```cpp
#include <algorithm>
#include <queue>
#include <set>
#include <vector>

std::pair<int, int> exhaustive_search_closest_coin(
    const std::pair<int, int> &start,
    const std::set<std::pair<int, int>> &coins, int square_length) {
    const auto result = std::min_element(
        coins.begin(), coins.end(),
        [&](const std::pair<int, int> &left, const std::pair<int, int> &right) {
            return std::abs(start.first - left.first) +
                       std::abs(start.second - left.second) <
                   std::abs(start.first - right.first) +
                       std::abs(start.second - right.second);
        });
    return *result;
}

std::pair<int, int> bfs_search_closest_coin(
    const std::pair<int, int> &start,
    const std::set<std::pair<int, int>> &coins, int square_length) {
    const std::vector<int> dir = {-1, 0, 1, 0, -1};
    std::queue<std::pair<int, int>> q;
    q.push(start);
    std::vector<std::vector<int>> visited(square_length,
                                          std::vector<int>(square_length, 0));

    while (!q.empty()) {
        int n = q.size();
        while (n) {
            std::pair<int, int> cur = q.front();
            q.pop();
            n--;

            if (visited[cur.first][cur.second]) {
                continue;
            }

            visited[cur.first][cur.second] = 1;

            if (coins.count(cur)) {
                return cur;
            }

            for (int i = 1; i < dir.size(); i++) {
                int new_x = cur.first + dir[i - 1];
                int new_y = cur.second + dir[i];

                if (new_x >= 0 && new_x < square_length && new_y >= 0 &&
                    new_y < square_length) {
                    q.push({new_x, new_y});
                }
            }
        }
    }

    return {};
}
```
