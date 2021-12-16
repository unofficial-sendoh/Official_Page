# Leetcode 300. Longest Increasing Subsequence

[Explanation Video Link on Youtube](https://youtu.be/SJZ\_KlTeG-Y)

[B站英文解说视频链接](https://www.bilibili.com/video/BV1r3411y7LA)

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int num_size = nums.size();
        if (num_size == 0) {
            return 0;
        }
        
        int help_array_end_index = 0;
        
        for (int i = 0; i < num_size; i++) {
            auto it = lower_bound(nums.begin(), nums.begin() + help_array_end_index, nums[i]);
            
            if (it == nums.begin() + help_array_end_index) {
                nums[help_array_end_index] = nums[i];
                help_array_end_index++;
            }
            else {
                *it = nums[i];
            }
            
        }
        
        return help_array_end_index;
    }
};
```
