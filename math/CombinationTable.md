$perm\\_multi(l, r)$：求序列 $a[l...r]$ 多重集的全排列，时间复杂度： $O(r - l)$。

以下操作时间复杂度均为 $O(1)$。

$perm(n)$：求全排列即 $A^n_n$​。

$perm(n, m)$：求排列数 $A^n_m$。

$comb(n, m)$：求组合数 $C^n_m$。

$comb\\_nonadjacent(n, m)$：求不相邻的组合数。

$perm\\_staggered(n)$：求全错排列。

$perm\\_circular(n)$​：求圆全排列即 $Q^n_n$。

$perm\\_circular(n, m)$：求圆排列数 $Q^n_m$​。

$perm\\_circular\\_nonadjacent(n, m)$：求不相邻的圆组合数。

```C++
template<class Tp>
class CombinationTable {
private:
    vector<Tp> fact, finv;
public:
    CombinationTable(const int &n) : fact(n + 1), finv(n + 1) {
        fact[0] = 1;
        for (int i = 1; i <= n; i++) {
            fact[i] = fact[i - 1] * i;
        }
        finv[n] = fact[n].inv();
        for (int i = n - 1; ~i; i--) {
            finv[i] = finv[i + 1] * (i + 1);
        }
    }
    Tp perm(const int &n) {
        return n < 0 ? Tp() : fact[n];
    }
    Tp perm(const int &n, const int &m) {
        if (n < 0 || m < 0 || n < m) {
            return Tp();
        }
        return fact[n] * finv[n - m];
    }
    Tp comb(const int &n, const int &m) {
        return perm(n, m) * finv[m];
    }
    template <typename It>
    Tp perm_multi(const It &l, const It &r) {
        Tp res = fact[accumulate(l, r, Tp())];
        for (It it = l; it != r; it++) {
            res *= finv[*it];
        }
        return res;
    }
    Tp comb_nonadjacent(int n, int m) {
        return comb(n - m + 1, m);
    }
    Tp perm_staggered(int n) {
        static vector<Tp> res{1, Tp()};
        while (res.size() < n + 1) {
            res.push_back((res[res.size() - 2] + res.back()) * res.size() - 1);
        }
        return res[n];
    }
    Tp perm_circular(int n) { 
        return fact[n - 1]; 
    }
    Tp perm_circular(int n, int m) {
        return comb(n, m) * fact[m - 1];
    }
    Tp perm_circular_nonadjacent(int n, int m) {
        return comb_nonadjacent(n, m) - comb_nonadjacent(n - 4, m - 2);
    }
};
```
