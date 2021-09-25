# Leetcode 1060. Missing Element in Sorted Array

{% embed url="https://youtu.be/5jDpjERnUGY" %}

```cpp
class Solution {
public:
    int missingElement(vector<int>& nums, int k) {
        int begin_ptr = 0;
        int end_ptr = nums.size();
        
        function<int(int)> missing_num = [&](int index) {
            return nums[index] - nums[0] - index;
        };
        
        while (begin_ptr < end_ptr) {
            int mid_ptr = (end_ptr - begin_ptr) / 2 + begin_ptr;
            
            if (missing_num(mid_ptr) < k) {
                begin_ptr = mid_ptr + 1;
            }
            else {
                end_ptr = mid_ptr;
            }
        }
        
        return nums[begin_ptr - 1] + k - missing_num(begin_ptr - 1);
    }
};
```

