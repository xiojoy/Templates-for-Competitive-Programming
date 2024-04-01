$Matrix$ 用于实现矩阵的基本运算，包括矩阵和数与矩阵间的加减乘的运算。 $n$ 为矩阵 $a$ 的行数， $m$ 为矩阵 $a$ 的列数和矩阵 $b$ 的行数， $l$ 为矩阵 $b$ 的列数。

矩阵乘法（ $ab$ ）的时间复杂度： $O(nml)$，矩阵快速幂（ $a^k$）的时间复杂度： $O(n^3logk)$，其余运算的复杂度： $O(nm)$。

矩阵加减法应满足行列数分别相等，矩阵乘法应满足被乘矩阵的列数与乘矩阵的行数相等，矩阵快速幂应满足矩阵行列数相等。

$Matrix$ 支持直接输入和输出。

```C++
template<class Tp>
class Matrix {
public:
    int n, m;
    vector<vector<Tp>> val;
    Matrix() {}
    Matrix(const int &n) : n(n), m(n) {
        val.assign(n, vector<Tp>(n));
        for (int i = 0; i < n; i++) {
            val[i][i] = 1;
        }
    }
    Matrix(const vector<vector<Tp>> &val) : n(val.size()), m(val[0].size()) {
        this->val = val;
    }
    Matrix(const int &n, const int &m, const Tp &x = Tp()) : n(n), m(m) {
        val.assign(n, vector<Tp>(m, x));
    }
    Matrix& operator+=(const Tp &x) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                val[i][j] += x;
            }
        }
        return *this;
    }
    Matrix& operator-=(const Tp &x) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                val[i][j] -= x;
            }
        }
        return *this;
    }
    Matrix& operator*=(const Tp &x) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                val[i][j] *= x;
            }
        }
        return *this;
    }
    friend Matrix operator+(const Matrix &a, const Tp &b) { 
        return Matrix(a) += b; 
    }
    friend Matrix operator-(const Matrix &a, const Tp &b) { 
        return Matrix(a) -= b; 
    }
    friend Matrix operator*(const Matrix &a, const Tp &b) { 
        return Matrix(a) *= b; 
    }
    Matrix& operator+=(const Matrix &x) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                val[i][j] += x.val[i][j];
            }
        }
        return *this;
    }
    Matrix& operator-=(const Matrix &x) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                val[i][j] -= x.val[i][j];
            }
        }
        return *this;
    }
    Matrix& operator*=(const Matrix &x) {
        int l = x.m;
        Matrix a(n, l);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                for (int k = 0; k < l; k++) {
                    a.val[i][k] += val[i][j] * x.val[j][k];
                }
            }
        }
        *this = a;
        return *this;
    }
    friend Matrix operator+(const Matrix &a, const Matrix &b) { 
        return Matrix(a) += b; 
    }
    friend Matrix operator-(const Matrix &a, const Matrix &b) { 
        return Matrix(a) -= b; 
    }
    friend Matrix operator*(const Matrix &a, const Matrix &b) { 
        return Matrix(a) *= b; 
    }
    Matrix ksm(i64 k) {
        Matrix res(n), a(*this);
        while (k) {
            if (k & 1) {
                res *= a;
            }
            k >>= 1;
            a *= a;
        }
        return res;
    }
};
template<typename Tp>
ostream& operator<<(ostream &out, const Matrix<Tp> &x) {
    for (int i = 0; i < x.n; i++) {
        for (int j = 0; j < x.m; j++) {
            out << x.val[i][j] << " \n"[j == x.m - 1];
        }
    }
    return out;
}
template<typename Tp>
istream& operator>>(istream &in, Matrix<Tp> &x) {
    for (int i = 0; i < x.n; i++) {
        for (int j = 0; j < x.m; j++) {
            in >> x.val[i][j];
        }
    }
    return in;
}
```

例题：

[P1306](https://www.luogu.com.cn/problem/P1306)

[P1962](https://www.luogu.com.cn/problem/P1962)

[P1939](https://www.luogu.com.cn/problem/P1939)

[P8624](https://www.luogu.com.cn/problem/P8624)
