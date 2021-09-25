# Implement Shared Pointer

{% embed url="https://youtu.be/PtnhXSfmIU0" %}

{% embed url="https://www.bilibili.com/video/BV1pf4y1A7dC/" %}

```cpp
template<typename T>
class SharedPtr {
private:
    T* m_data = nullptr;
    int* m_count = nullptr;

    void clean_up();
public:
    // Disable default constructor.
    SharedPtr() = delete;

    // Constructor
    SharedPtr(T data) {
        m_data = new T(data);
        m_count = new int(1);
    }

    // Copy Constructor
    SharedPtr(const SharedPtr<T>& another_ptr);
    
    // Destructor
    ~SharedPtr() {
        clean_up();
    }

    // Move Constructor
    SharedPtr(SharedPtr<T>&& another_ptr);

    T& operator*() const{
        return *m_data;
    }

    T* operator->() const{
        return m_data;
    }
    
    // Copy Assignment
    SharedPtr<T>& operator=(const SharedPtr<T>& another_ptr);
    
    // Move Assignment
    SharedPtr<T>& operator=(SharedPtr<T>&& another_ptr);
};

template<typename T>
void SharedPtr<T>::clean_up() {
    if (nullptr != m_count) {
        (*m_count)--;

        if (*m_count == 0) {
            delete m_data;
            delete m_count;
        }
    }
}

template<typename T> 
SharedPtr<T>::SharedPtr(const SharedPtr<T>& another_ptr) {
    m_data = another_ptr.m_data;
    m_count = another_ptr.m_count;

    if (nullptr != another_ptr.m_data) {
        (*m_count)++;
    }
}

template<typename T> 
SharedPtr<T>::SharedPtr(SharedPtr<T>&& another_ptr) {
    m_data = another_ptr.m_data;
    m_count = another_ptr.m_count;
    // Clean up another pointer
    another_ptr.m_data = nullptr;
    another_ptr.m_count = nullptr;
}

template<typename T> 
SharedPtr<T>& SharedPtr<T>::operator=(const SharedPtr<T>& another_ptr) {
    clean_up();        
    m_count = another_ptr.m_count;
    m_data = another_ptr.m_data;

    if (nullptr != another_ptr.m_data) {
        (*m_count)++;
    }
    return *this;
}

template<typename T> 
SharedPtr<T>&  SharedPtr<T>::operator=(SharedPtr<T>&& another_ptr) {
    clean_up();        
    m_count = another_ptr.m_count;
    m_data = another_ptr.m_data;
    // Clean up another pointer
    another_ptr.m_data = nullptr;
    another_ptr.m_count = nullptr;
    return *this;
}
```

