$Bellman\\_Ford$ 用于在图中求某起点经过至多 $k$ 条边到所有点的最短距离（允许有负权边和负环）。 
$n、m$ 分别为图的点数和边数， $u$ 为起点， $d_i$ 表示点 $i$ 与点 $u$ 的当前最短距离， $st_i$ 表示点 $i$ 与点 $u$ 的最短距离是否确定。

$Bellman\\_Ford(adj,\ u)$：返回以 $u$ 为起点经过 $k$ 条边的最短距离数组 $d$，时间复杂度： $O(km)$。

```c++
auto Bellman_Ford = [&](const vector<vector<array<int, 2>>> &adj, int u)->vector<int> {
    int n = adj.size() - 1;
    vector<int> d(n + 1, INF);
    d[u] = 0;
    for (int i = 1; i <= k; i++) {
        vector<int> las = d;
        for (auto [u, v, w] : E) d[v] = min(d[v], las[u] + w);
    }
    return d;
};
```
