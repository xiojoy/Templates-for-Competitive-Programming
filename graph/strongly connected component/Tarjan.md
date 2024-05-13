强连通分量是指极大的连通子图。 $tarjan$ 可用于求图的所有强连通分量。 $n、m$ 分别为点数和边数， $dfn_i$ 表示点 $i$ 的时间戳， $low_i$ 表示点 $i$ 的所在的强连通分量中所有点的最小 $dfn$ 值， $scc_i$ 表示点 $i$ 属于哪个强连通分量。

$get\\_scc(adj)$：返回 $scc$ 数组，时间复杂度： $O(n + m)$。

```c++
auto get_scc = [&](const vector<vector<int>> &adj)->vector<int> {
    stack<int> stk;
    int cur = 0, sc = 0;
    vector<int> low(n + 1), dfn(n + 1), scc(n + 1);
    auto Tarjan = [&](auto self, int u)->void {
        dfn[u] = low[u] = ++cur, stk.push(u);
        for (int v : adj[u]) {
            if (!dfn[v]) self(self, v), low[u] = min(low[u], low[v]);
            else if (!scc[v]) low[u] = min(low[u], dfn[v]);
        }
        if (low[u] == dfn[u]) {
            scc[u] = ++sc;
            while (stk.top() != u) scc[stk.top()] = sc, stk.pop();
            stk.pop();
        }
    };
    for (int i = 1; i <= n; i++) if (!dfn[i]) Tarjan(Tarjan, i);
    return scc;
};
```
