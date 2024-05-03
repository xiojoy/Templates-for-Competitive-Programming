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
        n = t.size(), s.resize(n + 1);
        for (int i = 1; i <= n; i++) s[i] = t[i - 1];
        work();
    }
    SuffixArray(const vector<int> &t) {
        n = t.size() - 1, s = t;
        work();
    }
    void work() {
        sa.resize(n + 1), rk.resize(n + 1);
        for (int i = 1; i <= n; i++) sa[i] = i, rk[i] = s[i];
        int rank = *max_element(ALL(s));
        auto count_sort = [&](int m, int k) {
            vector<int> lsa = sa, cnt(m + 1);
            for (int i = 1; i <= n; i++) {
                int j = lsa[i] + k;
                cnt[rk[j > n ? 0 : j]]++;
            }
            for (int i = 1; i <= m; i++) cnt[i] += cnt[i - 1];
            for (int i = n; i >= 1; i--) {
                int j = lsa[i] + k;
                sa[cnt[rk[j > n ? 0 : j]]--] = lsa[i];
            }
        };
        for (int k = 1; k < n; k <<= 1) {
            count_sort(rank, k), count_sort(rank, 0);
            vector<int> las = rk;
            int j = 0;
            for (int i = 1; i <= n; i++) {
                if (las[sa[i]] == las[sa[i - 1]] && las[sa[i] + k] == las[sa[i - 1] + k]) rk[sa[i]] = j;
                else rk[sa[i]] = ++j;
            }
            if ((rank = j) == n) break;
        }
        h.resize(n + 1);
        for(int i = 1; i <= n; i++) rk[sa[i]] = i;
        for(int i = 1, k = 0; i <= n; i++) {
            if(rk[i] == 1) continue;
            if(k) k--;
            int j = sa[rk[i] - 1];
            while (i + k <= n && j + k <= n && s[i + k] == s[j + k]) k++;
            h[rk[i]] = k;
        }
        ST.work(h);
    }
    int lcp(int x, int y) {
        if (x == y) return n - x + 1;
        x = rk[x], y = rk[y];
        if (x > y) swap(x, y);
        return ST.query(x + 1, y); // 求区间最小值
    }
};
```

1. 寻找最小的循环移动位置

​	将字符串 $S$ 复制一份变成 $SS$ 就转化成了后缀排序问题。

2. 在字符串中找子串

​	任务是在线地在主串 $T$ 中寻找模式串 $S$。在线的意思是，我们已经预先知道知道主串 $T$，但是当且仅当询问时才知道模式串 $S$。我们可以先构造出 $T$ 的后缀数组，然后查找子串 $S$。若子串 $S$ 在 $T$ 中出现，它必定是 $T$ 的一些后缀的前缀。因为我们已经将所有后缀排序了，我们可以通过在 $p$ 数组中二分 $S$ 来实现。比较子串 $S$ 和当前后缀的时间复杂度为 $O(|S|)$，因此找子串的时间复杂度为 $O(|S|\log |T|)$。注意，如果该子串在 $T$ 中出现了多次，每次出现都是在 $p$ 数组中相邻的。因此出现次数可以通过再次二分找到，输出每次出现的位置也很轻松。

3. 从字符串首尾取字符最小化字典序

​	暴力做法就是每次最坏 $O(n)$ 地判断当前应该取首还是尾（即比较取首得到的字符串与取尾得到的反串的大小），只需优化这一判断过程即可。由于需要在原串后缀与反串后缀构成的集合内比较大小，可以将反串拼接在原串后，并在中间加上一个没出现过的字符（如 `#`，代码中可以直接使用空字符），求后缀数组，即可 $O(1)$ 完成这一判断。

4. 两子串最长公共前缀

​	$lcp(sa[i],sa[j])=\min\{h[i+1..j]\}$。感性理解：如果 $h$ 一直大于某个数，前这么多位就一直没变过；反之，由于后缀已经排好序了，不可能变了之后变回来。

5. 比较一个字符串的两个子串的大小关系

​	假设需要比较的是 $A=S[a..b]$ 和 $B=S[c..d]$ 的大小关系。若 $lcp(a, c)\ge\min(|A|, |B|)$， $A<B\iff |A|<|B|$，否则， $A<B\iff rk[a]< rk[c]$。

6. 不同子串的数目

​	子串就是后缀的前缀，所以可以枚举每个后缀，计算前缀总数，再减掉重复。「前缀总数」其实就是子串个数，为 $n(n+1)/2$。如果按后缀排序的顺序枚举后缀，每次新增的子串就是除了与上一个后缀的 $LCP$ 剩下的前缀。这些前缀一定是新增的，否则会破坏 $lcp(sa[i],sa[j])=\min\{h[i+1..j]\}$ 的性质。只有这些前缀是新增的，因为 $LCP$ 部分在枚举上一个前缀时计算过了。所以答案为： $\frac{n(n+1)}{2}-\sum\limits_{i=2}^nh[i]$。

7. 出现至少 $k$ 次的子串的最大长度

​	出现至少 $k$ 次意味着后缀排序后有至少连续 $k$ 个后缀以这个子串作为公共前缀。所以，求出每相邻 $k-1$ 个 $h$ 的最小值，再求这些最小值的最大值就是答案。

8. 是否有某字符串在文本串中至少不重叠地出现了两次

​	可以二分目标串的长度 $|s|$，将 $h$ 数组划分成若干个连续 $LCP$ 大于等于 $|s|$ 的段，对每个段求其中出现的数中最大和最小的下标，若这两个下标的距离满足条件，则一定有长度为 $|s|$ 的字符串不重叠地出现了两次。

9. 连续的若干个相同子串

​	我们可以枚举连续串的长度 $|s|$，按照 $|s|$ 对整个串进行分块，对相邻两块的块首进行 $LCP$ 与 $LCS$​ 查询。

例题：

[contest_41745_C](https://ac.nowcoder.com/acm/contest/41745/C)

[P2852](https://www.luogu.com.cn/problem/P2852)

[Div3_943_G](https://codeforces.com/contest/1968/problem/G2)
