# Leetcode 706. Design Hashmap

[Explanation Video Link on Youtube](https://youtu.be/WtW8gZrIVHE)

[B站英文解说视频链接](https://www.bilibili.com/video/BV1k34y1S7Xw)

```cpp
class MyHashMap {
private:
    vector<list<pair<int, int>>> container;
    
public:
    /** Initialize your data structure here. */
    MyHashMap() {
        container.resize(10000);
    }
    
    int hash(int key){
        return key % 10000;
    }
    
    /** value will always be non-negative. */
    void put(int key, int value) {
        int hash_key = hash(key);
        
        for (auto& v:container[hash_key]){
            if (v.first == key){
                v.second = value;
                return;
            }
        }
        
        container[hash_key].emplace_back(key, value);
    }
    
    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        int hash_key = hash(key);
        
        for (auto& v:container[hash_key]){
            if (v.first == key){
                return v.second;
            }
        }
        
        return -1;
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        int hash_key = hash(key);
        for (auto i = container[hash_key].begin(); i!= container[hash_key].end(); i++){
            if (i->first == key){
                container[hash_key].erase(i);
                return;
            }
        }
    }
};
```
