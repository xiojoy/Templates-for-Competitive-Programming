$HighPrecision$​ 包含了四种高精度的基本运算， $A$ 为大整数， $B$ 在除法中为 $int$，在其余三种运算中为大整数， $n$ 为 $A$ 的长度， $m$ 为 $B$ 的长度。

$add(A, B)$：返回 $A + B$，时间复杂度： $O(\max(n,m))$。

$sub(A, B)$：返回 $A - B$，时间复杂度： $O(\max(n,m))$​。

$mul(A, B)$：返回 $A \times B$，时间复杂度： $O(nm)$​。

$div(A, B)$：返回 $A \div B$，时间复杂度： $O(n)$。

```C++
class HighPrecision {
private:
    vector<int> stov(const string &A, const int &siz) {
        vector<int> a(siz);
        int n = A.size();
        for (int i = 0; i < n; i++) {
            a[i] = A[n - 1 - i] - '0';
        }
        return a;
    }

    string vtos(vector<int> &c, bool neg = false) {
        while (c.back() == 0 && c.size() > 1) {
            c.pop_back();
        }
        string C;
        for (int i : c) {
            C.push_back(i + '0');
        }
        if (neg) {
            C.push_back('-');
        }
        reverse(C.begin(), C.end());
        return C;
    }
public:
    string add(const string &A, const string &B) {
        int n = max(A.size(), B.size()) + 1;
        vector<int> a = stov(A, n), b = stov(B, n), c(n);
        int x = 0;
        for (int i = 0; i < n; i++) {
            c[i] = a[i] + b[i] + x;
            x = c[i] / 10;
            c[i] %= 10;
        }
        return vtos(c);
    }

    string sub(string A, string B) {
        bool neg = false;
        if (A.size() < B.size() || (A.size() == B.size() && A < B)) {
            neg = true;
            swap(A, B);
        }
        int n = max(A.size(), B.size());
        vector<int> a = stov(A, n), b = stov(B, n), c(n);
        for (int i = 0; i < n; i++) {
            if (a[i] < b[i]) {
                a[i] += 10;
                a[i + 1]--;
            }
            c[i] = a[i] - b[i];
        }
        return vtos(c, neg);
    }

    string mul(const string &A, const string &B) {
        int n = A.size(), m = B.size();
        vector<int> a = stov(A, n), b = stov(B, m), c(n + m);
        for (int i = 0; i < n; i++) {
            int x = 0;
            for (int j = 0; j < m; j++) {
                c[i + j] += a[i] * b[j] + x;
                x = c[i + j] / 10;
                c[i + j] %= 10;
            }
            c[i + m] = x;
        }
        return vtos(c);
    }

    string div(const string &A, const int &B, int &x) {
        x = 0;
        int n = A.size(), b = B;
        vector<int> a = stov(A, n), c(n);
        for (int i = n - 1; ~i; i--)  {
            c[i] = a[i] + 10 * x;
            x = c[i] % b;
            c[i] /= b;
        }
        return vtos(c);
    }
};
```

例题：

[HDU1250](http://acm.hdu.edu.cn/showproblem.php?pid=1250)

[HDU5920](http://acm.hdu.edu.cn/showproblem.php?pid=5920)

[HDU1261](http://acm.hdu.edu.cn/showproblem.php?pid=1261)
