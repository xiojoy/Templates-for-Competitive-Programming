$ST$ 表常用于求满足可重复贡献和结合律的区间询问值。 $n$ 表示数组长度， $lg[i]$ 表示 $\lfloor log_2i\rfloor$， $st[i][j]$ 表示区间 $[i, i+2^j-1]$ 的询问值。

$init(a)$：初始化，时间复杂度： $O(nlogn)$。

$query(l,r)$：求区间 $[l,r]$ 的询问值，时间复杂度： $O(1)$。

支持高效区间查询，不支持区间修改。

```C++
struct SparseTable {
    int n;
    vector<vector<int>> st;
    vector<int> lg;
    
    SparseTable() {}
    SparseTable(const vector<int> &a) {
        work(a);
    }

    void work(const vector<int> &a) {
        n = a.size() - 1;
        lg.resize(n + 1);
        st.assign(n + 1, vector<int>(20, INF)); // logn < 20
        for (int i = 0; 1 << i <= n; i++) {
            lg[1 << i] = i;
        }
        for (int i = 1; i <= n; i++) {
            if (!lg[i]) {
                lg[i] = lg[i - 1];
            }
            st[i][0] = a[i];
        }
        for (int j = 1; 1 << j <= n; j++) {
            for (int i = 1; i + (1 << j) - 1 <= n; i++) {
                st[i][j] = min(st[i][j - 1], st[i + (1 << j - 1)][j - 1]);
            }
        }
    }

    int query(int l, int r) {
        int k = lg[r - l + 1];
        return min(st[l][k], st[r - (1 << k) + 1][k]);
    }
};
```

例题：

[Div2_914_D2](https://codeforces.com/contest/1904/problem/D2)

