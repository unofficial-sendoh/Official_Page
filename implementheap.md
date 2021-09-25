# Implement a Max Heap Using Dynamic Array

{% embed url="https://youtu.be/GI7CriPmU\_M" %}

{% embed url="https://www.bilibili.com/video/BV1Uv411A7GU/" %}

```cpp
#include <vector>
#include <optional>
using namespace std;

template <typename T>
class MaxHeap {
public:
    MaxHeap() = delete;

    explicit MaxHeap(const T& padding_val) {
        m_container.push_back(padding_val);
    }

    void insert(const T& data) {
        m_container.push_back(data);
        int cur_index = m_container.size() - 1;

        while (cur_index > kPaddingIndex) {
            int parent_index = cur_index / 2;
            if (m_container[cur_index] > m_container[parent_index]) {
                swap(m_container[cur_index], m_container[parent_index]);
            } else {
                break;
            }

            cur_index = parent_index;
        }
    }

    optional<T> top() {
        int top_element_index = kPaddingSize;
        return static_cast<int>(m_container.size()) > kPaddingSize ? optional<T>(m_container[top_element_index]) : nullopt;
    }

    void pop() {
        int back_element_index = m_container.size() - kPaddingSize;
        int top_element_index = kPaddingSize;
        swap(m_container[top_element_index], m_container[back_element_index]);
        m_container.pop_back();
        heapify(top_element_index);
    }

private:
    const int kPaddingSize = 1;
    const int kPaddingIndex = 1;
    vector<T> m_container;

    void heapify(int index) {
        int container_size = m_container.size();
        int left_index = index * 2;
        int right_index = index * 2 + 1;
      
        if (left_index < container_size && right_index < container_size && m_container[index] < m_container[right_index] && m_container[left_index] < m_container[right_index]) {
            swap(m_container[right_index], m_container[index]);
            heapify(right_index);
            return;
        }
            
        if (left_index < container_size && m_container[index] < m_container[left_index]) {
            swap(m_container[left_index], m_container[index]);
            heapify(left_index);
        }

    }
};
```

