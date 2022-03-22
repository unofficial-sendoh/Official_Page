# Leetcode 729. My Calendar I

```cpp
// Author: Huahua
class MyCalendar {
public:
    MyCalendar() {}
    
    bool book(int start, int end) {        
        auto it = booked_.lower_bound(start);

        if (it != booked_.cend() && it->first < end)
            return false;        
        if (it != booked_.cbegin() && (--it)->second > start)
            return false;        
        booked_[start] = end;
        return true;
    }
private:
    map<int, int> booked_;
};

```
