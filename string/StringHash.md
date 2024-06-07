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
