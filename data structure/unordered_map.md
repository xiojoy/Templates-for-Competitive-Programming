$unordered\\_map$ 的单次操作的期望时间复杂度为 $O(1)$， 但由于 $hash$ 函数固定易被卡至 $O(n)$，而使用自定义随机哈希函数 $custom\\_hash$ 防止被卡。

```C++
struct custom_hash {
    static uint64_t splitmix64(uint64_t x) {
        x += 0x9e3779b97f4a7c15;
        x = (x ^ (x >> 30)) * 0xbf58476d1ce4e5b9;
        x = (x ^ (x >> 27)) * 0x94d049bb133111eb;
        return x ^ (x >> 31);
    }

    size_t operator()(uint64_t x) const {
        static const uint64_t FIXED_RANDOM = chrono::steady_clock::now().time_since_epoch().count();
        return splitmix64(x + FIXED_RANDOM);
    }
};
unordered_map<int, int, custom_hash> mp;
```
