$Z$ 函数用于求字符串 $s$ 的所有后缀（ $0-index$）与 $s$  的 $lcp$， $n$ 为 $s$ 的长度， $z_i$ 表示后缀 $i$ 与 $s$ 的 $lcp$。

时间复杂度： $O(n)$。

```C++
vector<int> ZFunction(const string &s) {
    int n = s.size();
    vector<int> z(n + 1);
    z[0] = n;
    for (int i = 1, j = 1; i < n; i++) {
        z[i] = max(0, min(j + z[j] - i, z[i - j]));
        while (i + z[i] < n && s[z[i]] == s[i + z[i]]) z[i]++;
        if (i + z[i] > j + z[j]) j = i;
    }
    return z;
}
```

