# Lintcode 556. Standard Bloom Filter

[Explanation Video Link on Youtube](https://youtu.be/IlfoXC2OPLI)

[B站中文解说视频链接](https://www.bilibili.com/video/BV1ZR4y1t7h2)

```cpp
class HashFunction {
public:
    HashFunction(int cap, int seed) {
        m_cap = cap;
        m_seed = seed;
    }

    int hash(string key) {
        int res = 0;

        for (char &c : key) {
            res = res * m_seed + c;
            res = res % m_cap;
        }

        return res;
    }
private:
    int m_cap;
    int m_seed;
};

class StandardBloomFilter {
public:
    StandardBloomFilter(int k) {
        m_k = k;
        m_hash_funcs.reserve(m_k);
        const int kBitSize = 10000;

        for (int i = 0; i < m_k; i++) {
            // all primes (>3) are of the form 6i + 1 or 6i - 1
            m_hash_funcs.push_back(make_unique<HashFunction>(kBitSize, 6 * i + 1));
        }

        m_bit_array = vector<bool>(kBitSize, false);
    }

    void add(string &word) {
        for (int i = 0; i < m_k; i++) {
            int index = m_hash_funcs[i]->hash(word);
            m_bit_array[index] = true;
        }
    }

    bool contains(string &word) {
        for (int i = 0; i < m_k; i++) {
            int index = m_hash_funcs[i]->hash(word);
            if (!m_bit_array[index]) {
                return false;
            }
        }
        return true;
    }
private:
    int m_k;
    vector<unique_ptr<HashFunction>> m_hash_funcs;
    vector<bool> m_bit_array;
};
```
