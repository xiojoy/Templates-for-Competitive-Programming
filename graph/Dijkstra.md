$Dijkstra$ 用于在无负权边的图中求某起点到所有点的最短距离。 $n、m$ 分别为图的点数和边数， $u$ 为起点， $d_i$ 表示点 $i$ 与点 $u$ 的当前最短距离， $st_i$ 表示点 $i$ 与点 $u$ 的最短距离是否确定。

$Dijkstra(u)$： 返回以 $u$ 为起点的所有点到 $u$ 的最短距离数组，时间复杂度： $O((n+m)logn)$。

```c++
auto Dijkstra = [&](int u)->vector<int> {
    vector<int> d(n + 1, INF);
    vector<bool> st(n + 1);
    priority_queue<array<int, 2>, vector<array<int, 2>>, greater<array<int, 2>>> q; 
    d[u] = 0, q.push({0, u});
    while (!q.empty()) {
        auto [dis, u] = q.top(); 
        q.pop();
        if (st[u]) continue;
        st[u] = true;
        for (auto [v, w] : adj[u]) {
            if (d[v] > dis + w) {
                d[v] = dis + w;
                q.push({d[v], v});
            }
        }
    }
    return d;
};
```
