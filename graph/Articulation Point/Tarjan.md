强连通分量是指极大的连通子图。 $Tarjan$ 可用于求无向图的割点。

$get\\_ap(adj)$：返回 $ap$ 数组，时间复杂度： $O(n + m)$。
```c++
auto get_ap = [&](const vector<vector<int>> &adj)->vector<int> {
    int n = adj.size() - 1, cur = 0;
    vector<int> low(n + 1), dfn(n + 1), ap(n + 1);
    auto Tarjan = [&](auto self, int u, int fa)->void {
        dfn[u] = low[u] = ++cur;
        int ch = 0;
        for (int v : adj[u]) {
            if (!dfn[v]) {
                ch++, self(self, v, u), low[u] = min(low[u], low[v]);
                if (fa != u && low[v] >= dfn[u]) ap[u] = true;
            }
            else if (v != fa) low[u] = min(low[u], dfn[v]);
        }
        if (fa == u && ch >= 2) ap[u] = true;
    };
    for (int i = 1; i <= n; i++) if (!dfn[i]) Tarjan(Tarjan, i, i);
    return ap;
};
```
