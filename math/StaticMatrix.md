$StaticMatrix$ 用于实现矩阵的相关运算和操作， $M$ 为矩阵 $m$ 的行数， $N$ 为矩阵 $m$ 的列数和矩阵 $p$ 的行数， $L$ 为矩阵 $p$ 的列数。

$m.row(a)$，返回一个行列数同 $m$ 且全部元素值为 $a$ 的矩阵，时间复杂度： $O(MN)$。

$m.unit(a)$，返回一个行列数同 $m$ 的单位矩阵，时间复杂度： $O(MN)$。

$m.pow(n)$，快速幂返回 $m^n$，时间复杂度： $O(logn)$。

其余运算均与线性代数中的释义相同，矩阵乘法 $(m \times l)$ 的时间复杂度： $O(MNL)$，其余运算的时间复杂度： $O(MN)$。

矩阵运算在线性代数中应满足的前提条件均需满足。

```C++
template <typename Tp, uint32_t M, uint32_t N>
struct StaticMatrix {
    Tp m_val[M][N];
    static constexpr uint32_t row() { return M; }
    static constexpr uint32_t column() { return N; }
    static StaticMatrix<Tp, M, N> raw(Tp a) {
        StaticMatrix<Tp, M, N> res;
        for (uint32_t i = 0; i != M; i++)
            for (uint32_t j = 0; j != N; j++) res[i][j] = a;
        return res;
    }
    static StaticMatrix<Tp, M, M> unit() {
        static_assert(M == N);
        StaticMatrix<Tp, M, N> res{};
        for (uint32_t i = 0; i != M; i++) res[i][i] = 1;
        return res;
    }
    Tp *operator[](uint32_t i) { return m_val[i]; }
    const Tp *operator[](uint32_t i) const { return m_val[i]; }
    StaticMatrix<Tp, M, N> &operator+=(Tp a) {
        for (uint32_t i = 0; i != M; i++)
            for (uint32_t j = 0; j != N; j++) m_val[i][j] += a;
        return *this;
    }
    StaticMatrix<Tp, M, N> &operator-=(Tp a) {
        for (uint32_t i = 0; i != M; i++)
            for (uint32_t j = 0; j != N; j++) m_val[i][j] -= a;
        return *this;
    }
    StaticMatrix<Tp, M, N> &operator*=(Tp a) {
        for (uint32_t i = 0; i != M; i++)
            for (uint32_t j = 0; j != N; j++) m_val[i][j] *= a;
        return *this;
    }
    StaticMatrix<Tp, M, N> &operator+=(const StaticMatrix<Tp, M, N> &rhs) {
        for (uint32_t i = 0; i != M; i++)
            for (uint32_t j = 0; j != N; j++) m_val[i][j] += rhs[i][j];
        return *this;
    }
    StaticMatrix<Tp, M, N> &operator-=(const StaticMatrix<Tp, M, N> &rhs) {
        for (uint32_t i = 0; i != M; i++)
            for (uint32_t j = 0; j != N; j++) m_val[i][j] -= rhs[i][j];
        return *this;
    }
    StaticMatrix<Tp, M, M> pow(uint64_t n) const {
        static_assert(M == N);
        auto res = unit(), a = *this;
        while (n) {
            if (n & 1) res = res * a;
            if (n >>= 1) a = a * a;
        }
        return res;
    }
    template <typename Fp>
    friend StaticMatrix<Tp, M, N> operator+(const StaticMatrix<Tp, M, N> &a, const Fp &b) { return StaticMatrix<Tp, M, N>(a) += b; }
    template <typename Fp>
    friend StaticMatrix<Tp, M, N> operator-(const StaticMatrix<Tp, M, N> &a, const Fp &b) { return StaticMatrix<Tp, M, N>(a) -= b; }
    friend StaticMatrix<Tp, M, N> operator*(const StaticMatrix<Tp, M, N> &a, const Tp &b) { return StaticMatrix<Tp, M, N>(a) *= b; }
    template <uint32_t L>
    friend StaticMatrix<Tp, M, L> operator*(const StaticMatrix<Tp, M, N> &a, const StaticMatrix<Tp, N, L> &b) {
        StaticMatrix<Tp, M, L> res{};
        for (uint32_t i = 0; i != M; i++)
            for (uint32_t j = 0; j != N; j++) {
                Tp x = a.m_val[i][j];
                for (uint32_t k = 0; k != L; k++) res[i][k] += x * b[j][k];
            }
        return res;
    }
    friend bool operator==(const StaticMatrix<Tp, M, N> &a, const StaticMatrix<Tp, M, N> &b) {
        for (uint32_t i = 0; i != M; i++)
            for (uint32_t j = 0; j != N; j++)
                if (a[i][j] != b[i][j]) return false;
        return true;
    }
    friend bool operator!=(const StaticMatrix<Tp, M, N> &a, const StaticMatrix<Tp, M, N> &b) {
        for (uint32_t i = 0; i != M; i++)
            for (uint32_t j = 0; j != N; j++)
                if (a[i][j] != b[i][j]) return true;
        return false;
    }
};
```

例题：

[P1939](https://www.luogu.com.cn/problem/P1939)	

[P1962](https://www.luogu.com.cn/problem/P1962)
