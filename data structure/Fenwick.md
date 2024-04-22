树状数组所维护的数组记为 $a$ ， $n$ 为 $a$ 中元素的个数， $lowbit(i)$ 表示最低位 $1$ 和后面所有 $0$ 组成的数， $c[i]$ 表示 $\sum_{j=i-lowbit(i)+1}^ia[j]$ 的和。

$add(k, x)$：单点修改，表示 $a[k]=a[k]+x$，时间复杂度： $O(logn)$。

$sum$：区间查询， $sum(k)$ 表示 $\sum_{j=1}^{k}a[j]$，$sum(l, r)$表示$\sum_{j=l}^{r}a[j]$，时间复杂度：$O(logn)$。

$kth(k)$：求第 $k$ 小的数，此时 $a$ 应为权值数组，即 $a[i]$ 表示数 $i$ 出现的次数，时间复杂度：$O(logn)$。

```c++
template <typename T>
struct Fenwick {
    int n;
    std::vector<T> c;
    #define lowbit(x) (x & -x)
    Fenwick(int n) : n(n - 1), c(n) {}
    void add(int k, T x) { for (int i = k; i <= n; i += lowbit(i)) c[i] += x; }
    T sum(int k) {
        T sum = T();
        for (int i = k; i; i -= lowbit(i)) sum += c[i];
        return sum;
    }
    T sum(int l, int r) { return sum(r) - sum(l - 1); }
    int kth(T k) {
        int x = 0;
        for (int i = 1 << __lg(n); i; i >>= 1)
            if (x + i <= n && k > c[x + i]) x += i, k -= c[x];
        return x + 1;
    }
};
```
