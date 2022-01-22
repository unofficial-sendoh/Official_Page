# ID Allocator

```cpp
#include <unordered_set>
#include <queue>
#include <limits>

class IdAllocater {
private:
  std::unordered_set<int> allocated_ids_;
  std::queue<int> available_ids_queue_;
  int new_id_start_ = 0;
  constexpr static int kMaxValue_ = std::numeric_limits<int>::max();

public:
  int allocate() {
    if (!available_ids_queue_.empty()) {
      int id = available_ids_queue_.front();
      available_ids_queue_.pop();
      allocated_ids_.insert(id);
      return id;
    }

    if (new_id_start_ == kMaxValue_) {
      return -1;
    }

    allocated_ids_.insert(new_id_start_);
    return new_id_start_++;
  }

  bool release(int id) {
    if (!allocated_ids_.count(id)) {
      return false;
    }

    allocated_ids_.erase(id);
    available_ids_queue_.push(id);
    return true;
  } 
};
```

```cpp
#include <bitset>
#include <limits>
#include <mutex>

class SpacialEfficientIdAllocater {
public:
  SpacialEfficientIdAllocater() {
     ids_state_.reset();
  }

  int allocate() {
    const std::lock_guard<std::mutex> lock(mutex_);

    for (int i = 0; i < ids_state_.size(); i++) {
      if (!ids_state_[i]) {
        ids_state_.set(i, true);
        return i;
      }
    }
    
    return -1;
  }

  bool release(int id) {
    if (id < 0) {
      return false;
    }
    
    const std::lock_guard<std::mutex> lock(mutex_);
    ids_state_.set(id, false);
    return true;
  }
  
private:
  std::bitset<std::numeric_limits<int>::digits> ids_state_;
  std::mutex mutex_;
};
```

```cpp
#include <bitset>

class BinaryHeapIdAllocater {
private:
  constexpr static int kMaxID_ = 32768;  
  constexpr static int kMaxIndex_ = kMaxID_ * 2; 
  std::bitset<kMaxIndex_> ids_state_;
  int root_index_ = 1;

  int get_index_from_id(int id) {
    if (id < 0 || id >= kMaxID_) {
        return -1;
    }
    return kMaxID_ - id;
  }

  int get_id_from_index(int index) {
    return index - kMaxID_;
  }

  void update_tree(int cur_index) {
    while (cur_index >= root_index_) {
      cur_index = cur_index / 2;
      int left_child_index = cur_index * 2;
      int right_child_index = cur_index * 2 + 1;

      if (ids_state_[left_child_index] && ids_state_[right_child_index]) {
         ids_state_.set(cur_index, true);
      }
      else {
        ids_state_.set(cur_index, false);
      }
    }
  }
  
public:
  BinaryHeapIdAllocater() {
    ids_state_.reset();
  }
  int allocate() {
    if (ids_state_[root_index_]) {
      return -1;
    }
      
    int cur_index = root_index_;
    int target_index = cur_index;

    while (cur_index * 2 < kMaxIndex_) {
      int left_child_index = cur_index * 2;
      int right_child_index = cur_index * 2 + 1;

      if (!ids_state_[left_child_index]) {
        cur_index = left_child_index;
      }
      else if (!ids_state_[right_child_index]) {
        cur_index = right_child_index;
      }
      else {
        return -1;
      }

    }
      
    int id = get_id_from_index(cur_index);
    ids_state_.set(cur_index, true);
    update_tree(cur_index);
    return id;
  }

  bool release(int id) {
    int index = get_index_from_id(id);

    if (id < 0) {
      return false;
    }

    ids_state_.set(index, false);
    update_tree(index);
    return true;
  } 
};
```
