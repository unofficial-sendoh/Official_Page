# Leetcode 1891.  Cutting Ribbons

```cpp
class Solution {
public:
    int maxLength(vector<int>& ribbons, int k) {
        long long sum = std::accumulate(ribbons.begin(), ribbons.end(), 0LL);
        int right_ptr = sum / k + 1;
        int left_ptr = 0;
        
        while (left_ptr < right_ptr) {
            int mid_ptr = (right_ptr - left_ptr) /2 + left_ptr;
            
            if (is_possible(mid_ptr, ribbons, k)) {
                left_ptr = mid_ptr + 1;
            }
            else {
                right_ptr = mid_ptr;
            }
        }
        
        return left_ptr - 1;
    }
    
    bool is_possible(int len, const vector<int>& ribbons, int k) {
        if (len == 0) return true;
        int accumulate_length = 0;
        
        for (const auto& ribbon : ribbons) {
            accumulate_length += ribbon / len;
            
            if (accumulate_length >= k) {
                return true;
            }
        }
        
        return false;
    }
};
```
