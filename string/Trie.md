字典树能以树的形式用较小的空间存储所有字符串， $t[k][u]$ 表示节点 $k$ 且出边为 $u$ 的另一侧节点编号， $val[i]$ 表示以节点 $i$ 结束的字符串出现的次数，代码中为两种常见的字典树（分别存储了由小写字母构成的字符串和 $unsigned\ int$ 数据类型的数）。

$insert(s)$：插入字符串 $s$，时间复杂度： $O(|s|)$。

$query(s)$：查询相关信息，时间复杂度： $O(|s|)$。

```C++
struct Trie { 
    vector<array<int, 26>> t;
    vector<int> val;
    Trie() : t(1), val(1) {}
    void insert(const string &s, int p = 1) {
        int k = 0;
        for (char c : s) {
            int u = c - 'a';
            if (!t[k][u]) {
                t[k][u] = t.size();
                t.push_back(array<int, 26>());
                val.push_back(0);
            }
            k = t[k][u];
        }
        val[k] += p;
    };
    int query(const string &s) {
        int k = 0;
        for (char c : s) {
            int u = c - 'a';
            if (!t[k][u]) {
                t[k][u] = t.size();
                t.push_back(array<int, 26>());
                val.push_back(0);
            }
            k = t[k][u];
        }
        return val[k];
    }
};
struct Trie {
    vector<array<int, 2>> t;
    vector<int> val;
    Trie() : t(1), val(1) {}
    void insert(const int &x, int p = 1) {
        int k = 0;
        for (int i = 30; i >= 0; i--) {
            int u = x >> i & 1;
            if (!t[k][u]) {
                t[k][u] = t.size();
                t.push_back(array<int, 2>());
                val.push_back(0);
            }
            k = t[k][u];
        }
        val[k] += p;
    };
    int query(int x) {
        int k = 0;
        for (int i = 30; i >= 0; i--) {
            int u = x >> i & 1;
            if (t[k][!u]) u = !u;
            x ^= u << i;
            k = t[k][u];
        }
        return x;
    }
};
```

1. 检索字符串

2. 维护异或极值

例题：

[Div2_173_E](https://codeforces.com/problemset/problem/282/E)

[Div3_950_G](https://codeforces.com/contest/1980/problem/G)

[Div2_815_D2](https://codeforces.com/contest/1720/problem/D2)
