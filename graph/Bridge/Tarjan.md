
```c++
auto get_bge = [&](const vector<vector<int>> &adj)->vector<array<int, 2>> {
    int n = adj.size() - 1, cur = 0;
    vector<bool> isbge(n + 1);
    vector<int> low(n + 1), dfn(n + 1), f(n + 1);
    vector<array<int, 2>> bge;
    auto Tarjan = [&](auto self, int u, int fa)->void {
        f[u] = fa;
        dfn[u] = low[u] = ++cur;
        for (int v : adj[u]) {
            if (!dfn[v]) {
                self(self, v, u), low[u] = min(low[u], low[v]);
                if (low[v] > dfn[u]) isbge[v] = true;
            }
            else if (v != fa) low[u] = min(low[u], dfn[v]);
        }
    };
    for (int i = 1; i <= n; i++) if (!dfn[i]) Tarjan(Tarjan, i, 0);
    for (int i = 1; i <= n; i++) if (isbge[i]) bge.push_back({f[i], i});
    return bge;
};
```

例题：

[Div3_954_F](https://codeforces.com/contest/1986/problem/F)
