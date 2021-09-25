# Leetcode 1838. Frequency of the Most Frequent Element

{% embed url="https://youtu.be/2F255fcYdyk" %}

{% embed url="https://www.bilibili.com/video/BV1844y187z9/" %}

```cpp
class Solution {
public:
    int maxFrequency(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int max_freq = 1;
        int left_ptr = 0;
        int n = nums.size();
        long actual_sum = nums[left_ptr];
        
        for (int right_ptr = 1; right_ptr < n; right_ptr++) {
            long target_sum = (right_ptr - left_ptr + 1) * static_cast<long>(nums[right_ptr]);
            actual_sum += nums[right_ptr];
            
            while (target_sum - actual_sum > k) {
                target_sum -= nums[right_ptr];
                actual_sum -= nums[left_ptr];
                left_ptr++;
            }
            
            max_freq = max(max_freq, right_ptr - left_ptr + 1);
        }
        
        return max_freq;
    }
};
```

