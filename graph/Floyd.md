$Floyd$ 用于求任意两点之间的最短距离（不允许有负环）。 $d_{i,j}$ 表示点 $i$ 到点 $j$ 的最短距离。

$Floyd()$：返回最短距离数组 $d$，时间复杂度： $O(n^3)$。

```c++
auto Floyd = [&]()->vector<vector<int>> {
    vector<vector<int>> d(n + 1, vector<int>(n + 1, INF));
    for (int i = 1; i <= n; i++) d[i][i] = 0;
    for (auto [u, v, w] : E) d[u][v] = min(d[u][v], w);
    for (int k = 1; k <= n; k++)
        for (int i = 1; i <= n; i++)
            for (int j = 1; j <= n; j++)
                d[i][j] = min(d[i][j], d[i][k] + d[k][j]);
    return d;
};
```
