# Leetcode 715. Range Module

{% embed url="https://youtu.be/LV_KASL1smE" %}

```cpp
// Credit to Huahua's solution
class RangeModule {
private:
    map<int, int> m_intervals;
    
    bool get_overlapped_region(int left, int right, map<int, int>::iterator& it_left, map<int, int>::iterator& it_right){
        it_left = m_intervals.upper_bound(left);
        it_right = m_intervals.upper_bound(right);

        if (it_left != m_intervals.begin()) {
            it_left--;

            if (it_left->second < left) {
                it_left++;
            }
        }
        
        return it_left != it_right;
    }
    
public:
    RangeModule() {
        
    }
    
    void addRange(int left, int right) {
        map<int, int>::iterator it_left = m_intervals.end();
        map<int, int>::iterator it_right = m_intervals.end();
        
        if (get_overlapped_region(left, right, it_left, it_right)) {
            auto it_last = std::prev(it_right);
            left = min(left, it_left->first);
            right = max(right, it_last->second);
            m_intervals.erase(it_left, it_right);
        }
        
        m_intervals[left] = right;
    }
    
    bool queryRange(int left, int right) {
        map<int, int>::iterator it_left = m_intervals.end();
        map<int, int>::iterator it_right = m_intervals.end();
        
        if (!get_overlapped_region(left, right, it_left, it_right)) {
            return false;
        }
        
        return it_left->first <= left && it_left->second >= right;
    }
    
    void removeRange(int left, int right) {
        map<int, int>::iterator it_left = m_intervals.end();
        map<int, int>::iterator it_right = m_intervals.end();
        get_overlapped_region(left, right, it_left, it_right);
        
        if (!get_overlapped_region(left, right, it_left, it_right)) {
            return;
        }
        
        auto it_last = std::prev(it_right);
        int start = min(left, it_left->first);
        int end = max(right, it_last->second);
        m_intervals.erase(it_left, it_right);
        if (start < left) m_intervals[start] = left;
        if (end > right) m_intervals[right] = end;
    }
};
```
