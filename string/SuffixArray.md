后缀数组包括 $sa$ 和 $rk$，简记字符串 $s$ 的某个后缀为该后缀首字符的下标（从 $1$ 开始）， $sa[i]$ 表示在 $s$ 所有的后缀中排名为 $i$ （第 $i$ 小）的后缀， $rk[i]$ 表示后缀 $i$​ 的排名， $n$ 为 $s$ 的长度。

求后缀数组的时间复杂度： $O(nlogn)$ 。

```C++
    int n = s.size();
    s = " " + s;

    vector<int> sa(n + 1), rk(n + 1);
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
```

