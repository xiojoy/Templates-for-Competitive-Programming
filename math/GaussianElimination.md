高斯消元法可用于求解矩阵的秩和如下的 $n$ 元线性一次方程组。

$$ \begin{cases} a_{1, 1} x_1 + a_{1, 2} x_2 + \cdots + a_{1, n} x_n = b_1 \\ a_{2, 1} x_1 + a_{2, 2} x_2 + \cdots + a_{2, n} x_n = b_2 \\ \cdots \\ a_{n,1} x_1 + a_{n, 2} x_2 + \cdots + a_{n, n} x_n = b_n \end{cases}$$

记矩阵的秩为 $r$ （代码中为 $row$），当 $r < n$ 时有无穷多组解和无解两种情况，具体为哪种见代码，当 $r = n$ 时有唯一一组解，解存在矩阵的最后一列，时间复杂度： $O(n^3)$。

```C++
const double eps = 1e-8;
state gaussian_elimination(vector<vector<double>> &v) {
    int n = v.size(), col, row;
    for (col = row = 0; col < n; col++) {
        int pr = row;
        for (int i = row; i < n; i++) 
            if (fabs(v[i][col]) > fabs(v[pr][row])) pr = i;
        if (fabs(v[pr][col]) < eps) continue;
        for (int i = col; i <= n; i++) swap(v[pr][i], v[row][i]);
		for (int i = n; i >= col; i--) v[row][i] /= v[row][col];
        for (int i = row + 1; i < n; i++) {
            if (fabs(v[i][col]) < eps) continue;
            for (int j = n; j >= col; j--) v[i][j] -= v[row][j] * v[i][col];
        }
        row++;
    }
    if (row < n) {
        for (int i = row; i < n; i++) 
            if (fabs(v[i][n]) > eps) return No_solution;
        return Infinite_group_solutions;
    }
    for (int i = n - 1; i >= 0; i--)
        for (int j = i + 1; j < n; j++) v[i][n] -= v[i][j] * v[j][n];
    return Unique_solution;
}
```

