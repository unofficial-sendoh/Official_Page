# Leetcode 2461. Maximum Sum of Distinct Subarrays With Length K

```cpp
class Solution {
public:
    long long maximumSubarraySum(vector<int>& nums, int k) {
        int n = nums.size();
        if (n < k) return 0;
        if (k == 1) return *std::max_element(nums.begin(), nums.end());
        int left_ptr = 0;
        long long res = 0;
        long long max_sum = nums.front();
        unordered_set<int> visited;
        visited.insert(nums.front());

        for (int right_ptr = 1; right_ptr < n; right_ptr++) {
            while (visited.count(nums[right_ptr])) {
                max_sum -= nums[left_ptr];
                visited.erase(nums[left_ptr]);
                left_ptr++;
            }

            visited.insert(nums[right_ptr]);
            max_sum += nums[right_ptr];

            if (right_ptr - left_ptr + 1 == k) {
                res = max(res, max_sum);
                max_sum -= nums[left_ptr];
                visited.erase(nums[left_ptr]);
                left_ptr++;
            }
        }

        return res;
    }
};
```
