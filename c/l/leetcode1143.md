# Leetcode 1143. Longest Common Subsequence

{% embed url="https://youtu.be/az-\_\_D73uP4" %}

```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int m = text1.size();
        int n = text2.size();
        vector<int> dp(m + 1, 0);
        
        for (int i = 0; i < n; i++) {
            vector<int> prev_dp = dp;
            
            for (int j = 0 ; j < m; j++) {
                if (text1[j] == text2[i]) {
                    dp[j + 1] = prev_dp[j] + 1;
                }
                else{
                    dp[j + 1] = max(prev_dp[j + 1], dp[j]);
                }
            }
        }
        
        return dp.back();
    }
};
```

