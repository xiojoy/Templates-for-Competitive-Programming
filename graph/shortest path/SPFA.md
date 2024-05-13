$SPFA$ 用于在图中求某起点到所有点的最短距离（允许有负权边，不允许有负环）和判断负环（额外记录起点到每个点的最短路径上的边数）。 
$n、m$ 分别为图的点数和边数， $u$ 为起点， $d_i$ 表示点 $i$ 与点 $u$ 的当前最短距离， $st_i$ 表示点 $i$ 是否在队列中。

$SPFA(u)$：返回以 $u$ 为起点的最短距离数组 $d$，时间复杂度：一般为 $O(m)$，最坏会被卡至 $O(nm)$。

```C++
auto SPFA = [&](const vector<vector<array<int, 2>>> &adj, int u)->vector<int> {
    int n = adj.size() - 1;
    vector<int> d(n + 1, INF);
    vector<bool> st(n + 1);
    queue<int> q; 
    d[u] = 0, q.push(u), st[u] = true;
    while (!q.empty()) {
        int u = q.front(); 
        q.pop();
        st[u] = false;
        for (auto [v, w] : adj[u]) {
            if (d[v] > d[u] + w) {
                d[v] = d[u] + w;
                if (!st[v]) q.push(v), st[v] = true; 
            }
        }
    }
    return d;
};
```
