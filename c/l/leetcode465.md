# Leetcode 465. Optimal Account Balancing

{% embed url="https://youtu.be/pnB3N7rIB_w" %}

{% embed url="https://www.bilibili.com/video/BV1qF411Y7p9" %}

```cpp
class Solution {
public:
    int minTransfers(vector<vector<int>>& transactions) {
        unordered_map<int, long> index_to_balance;
        
        for (const auto& transaction : transactions) {
            index_to_balance[transaction[0]] -= transaction[2];
            index_to_balance[transaction[1]] += transaction[2];
        }
        
        vector<long> balance_list;
        
        for (const auto& pair : index_to_balance) {
            if (pair.second != 0) {
                balance_list.push_back(pair.second);
            }
        }
        
        return dfs(0, balance_list);
    }
    
private:
    int dfs(int index, vector<long> balance_list) {
        int n = balance_list.size();
        
        if (index == n) {
            return 0;
        }
        
        if (balance_list[index] == 0) {
            return dfs(index + 1, balance_list);
        }
        
        int min_transaction = INT_MAX;
        
        for (int i = index + 1; i < n; i++) {
            if (balance_list[index] * balance_list[i] < 0) {
                balance_list[i] += balance_list[index];
                min_transaction = min(min_transaction, 1 + dfs(index + 1, balance_list));
                
                if (balance_list[i] == 0) {
                    break;
                }
                
                balance_list[i] -= balance_list[index];
            }
        }
        
        return min_transaction;
    }
};
```
