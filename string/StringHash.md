字符串哈希通过对字符串（ $1-index$）中的每个字符进行哈希处理，从而将字符串的每个不同子串映射为不同的哈希值。

$init()$：初始化哈希结构，计算每个前缀的哈希值和基数的幂，时间复杂度： $O(n)$。

$query(l,r)$：求给定区间内的字符串的哈希值，时间复杂度： $O(1)$。

$same(l_1,r_1,l_2,r_2)$：比较两个区间内的字符串是否相同，时间复杂度： $O(1)$。

```c++
struct StringHash {
    static constexpr int B = 131;
    static constexpr array<int, 2> M = {1000000021, 1000000453};
    array<vector<int>, 2> h, b;
    StringHash(){}
    StringHash(const string &s) { init(s); }
    void init(const string &s) {
        int n = s.size();
        for (int j = 0; j < 2; j++) {
            h[j].resize(n + 1), b[j].resize(n + 1, 1);
        }
        for (int j = 0; j < 2; j++) {
            for (int i = 1; i <= n; i++) {
                b[j][i] = 1LL * b[j][i - 1] * B % M[j];
                h[j][i] = (1LL * h[j][i - 1] * B + s[i - 1]) % M[j];
            }
        }
    }
    array<int, 2> query(int l, int r) {
        array<int, 2> val;
        for (int j = 0; j < 2; j++) {
            val[j] = (h[j][r] - 1LL * h[j][l - 1] * b[j][r - l + 1] % M[j] + M[j]) % M[j];
        }
        return val;
    }
    bool same(int l1, int r1, int l2, int r2) {
        return query(l1, r1) == query(l2, r2);
    }
};
```
