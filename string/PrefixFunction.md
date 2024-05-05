前缀函数用于求字符串 $s$ 的所有前缀（ $0-index$）的 $border$， $n$ 为 $s$ 的长度， $pi_i$ 表示前缀 $i$ 的 $border$。

时间复杂度： $O(n)$。

```C++
vector<int> PrefixFunction(const string &s) {
    int n = s.size();
    vector<int> pi(n);
    for (int i = 1; i < n; i++) {
        int j = pi[i - 1];
        while (j && s[i] != s[j]) j = pi[j - 1];
        if (s[i] == s[j]) j++;
        pi[i] = j;
    }   
    return pi;
}
```
