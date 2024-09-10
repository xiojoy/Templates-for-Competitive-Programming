```cpp
template <typename T>
struct LinearBasis {
    vector<T> d, p;
    bool flag = false;

    LinearBasis(int k) {
        init(k);
    }

    void init(int k) {
        d.assign(k, T());
        flag = T();
    }
    bool insert(T x) {
        for (int i = d.size() - 1; ~i; i--) {
            if (x >> i & 1) {
                if (!d[i]) return d[i] = x;
                x ^= d[i];
            }
        }
        return !(flag = true);
    }
    T max() {
        T x = 0;
        for (int i = d.size() - 1; ~i; i--) {
            if (d[i] && ~x >> i & 1) x ^= d[i];
        }
        return x;
    }
    T min() {
        if (flag) {
            return 0;
        }
        for (int i = 0; i < d.size(); i++) {
            if (d[i]) return d[i];
        }
    }
    void rebuild() {
        for (int i = d.size() - 1; ~i; i--) {
            for (int j = i - 1; ~j; j--) {
                if (d[i] >> j & 1) d[i] ^= d[j];
            }
        }
        for (int i = 0; i < d.size(); i++) {
            if (d[i]) p.push_back(d[i]);
        }
    }
    T kth(i64 k) {
        k -= flag;
        if (!k) return 0;
        if (k >= T(1) << p.size()) return -1;
        T x = 0;
        for (int i = 0; i < p.size(); i++) {
            if (k >> i & 1) x ^= p[i];
        }
        return x;
    }
};

template <typename T>
LinearBasis<T> operator+(const LinearBasis<T> &a, const LinearBasis<T> &b) {
    LinearBasis<T> c(a);
    for (int i = c.d.size() - 1; ~i; i--) {
        if (b.d[i]) c.insert(b.d[i]);
    }
    return c;
}
```
