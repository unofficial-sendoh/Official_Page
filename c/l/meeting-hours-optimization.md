# Meeting Hours Optimization

```cpp
#include <vector>
#include <iostream>
#include <algorithm>

int meeting_hours_optimization(const std::vector<int> &meeting_hours, int hour_constraint)
{
    std::vector<int> helper(hour_constraint + 1, 0);

    for (const auto &hour : meeting_hours)
    {
        for (int j = hour_constraint; j > 0; j--)
        {
            if (helper[j] > 0 && j + hour <= hour_constraint)
            {
                helper[j + hour] = std::max(helper[j + hour], helper[j] + 1);
            }
        }

        helper[hour] = helper[hour] == 0 ? 1 : helper[hour];
    }

    return *std::max_element(helper.begin(), helper.end());
}

int main()
{
    std::vector<int> meeting_hours = {2, 2, 3, 4, 8};
    std::cout << meeting_hours_optimization(meeting_hours, 8) << std::endl;
}
```
