后缀数组包括 $sa$ 和 $rk$，简记字符串 $s$ 的某个后缀为该后缀首字符的下标（从 $1$ 开始）， $sa[i]$ 表示在 $s$ 所有的后缀中排名为 $i$ （第 $i$ 小）的后缀， $rk[i]$ 表示后缀 $i$ 的排名， $n$ 为 $s$ 的长度， $h[i]$ 表示 $lcp(sa[i],sa[i - 1])$， $ST$ 为 [SparseTable](https://github.com/xiojoy/Templates-for-Competitive-Programming/blob/main/data%20structure/SparseTable.md)，用于高效求 $lcp$。

$work()$：求 $sa、rk、h$，时间复杂度： $O(nlogn)$。

$lcp(x, y)$：求后缀 $x$ 和 $y$ 的最长公共前缀，时间复杂度： $O(1)$。

```C++
struct SuffixArray {
    int n;
    vector<int> s, sa, rk, h;
    SparseTable ST;
    
    SuffixArray(){}        
    SuffixArray(const string &t) {
        n = t.size();
        s.resize(n + 1);
        for (int i = 1; i <= n; i++) {
            s[i] = t[i - 1];
        }
        work();
    }
    SuffixArray(const vector<int> &t) {
        n = t.size() - 1;        
        s = t;
        work();
    }

    void work() {
        sa.resize(n + 1);
        rk.resize(n + 1);
        for (int i = 1; i <= n; i++) {
            sa[i] = i; 
            rk[i] = s[i];
        }
        int rank = *max_element(ALL(s));
        auto count_sort = [&](int m, int k) {
            vector<int> lsa = sa, cnt(m + 1);
            for (int i = 1; i <= n; i++) {
                int j = lsa[i] + k;
                cnt[rk[j > n ? 0 : j]]++;
            }
            for (int i = 1; i <= m; i++) {
                cnt[i] += cnt[i - 1];
            }
            for (int i = n; i >= 1; i--) {
                int j = lsa[i] + k;
                sa[cnt[rk[j > n ? 0 : j]]--] = lsa[i];
            }
        };

        for (int k = 1; k < n; k <<= 1) {
            count_sort(rank, k);
            count_sort(rank, 0);
            vector<int> las = rk;
            int j = 0;
            for (int i = 1; i <= n; i++) {
                if (las[sa[i]] == las[sa[i - 1]] && las[sa[i] + k] == las[sa[i - 1] + k]) {
                    rk[sa[i]] = j;
                } else {
                    rk[sa[i]] = ++j;
                }
            }
            rank = j;
            if (rank == n) {
                break;
            }
        }

        h.resize(n + 1);
        for(int i = 1; i <= n; i++) rk[sa[i]] = i;
        for(int i = 1, k = 0; i <= n; i++) {
            if(rk[i] == 1) continue;
            if(k) k --;
            int j = sa[rk[i] - 1];
            while (i + k <= n && j + k <= n && s[i + k] == s[j + k]) k++;
            h[rk[i]] = k;
        }
        ST.work(h);
    }

    int lcp(int x, int y) {
        if (x == y) {
            return n - x + 1;
        } else {
            x = rk[x];
            y = rk[y];
            if (x > y) {
                swap(x, y);
            }
            return ST.query(x + 1, y);
        }
    }
};
```

例题：

[contest_41745_C](https://ac.nowcoder.com/acm/contest/41745/C)

[P2852](https://www.luogu.com.cn/problem/P2852)

[Div3_943_G](https://codeforces.com/contest/1968/problem/G2)
