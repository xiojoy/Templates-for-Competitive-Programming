$Dijkstra$ 用于在无负权边的图中求某起点到所有点的最短距离。 $n、m$ 分别为图的点数和边数， $u$ 为起点， $d_i$ 表示点 $i$ 与点 $u$ 的当前最短距离， $st_i$ 表示点 $i$ 与点 $u$ 的最短距离是否确定。

$Dijkstra(adj,\ u)$：返回以 $u$ 为起点的最短距离数组 $d$，时间复杂度： $O((n+m)logn)$。

```c++
auto Dijkstra = [&](int u) {
    vector<bool> st(n + 1);
    vector<i64> d(n + 1, LLONG_MAX >> 1);
    priority_queue<tuple<i64, int>, vector<tuple<i64, int>>, greater<>> q; 
    d[u] = 0;
    q.push({0, u});
    while (!q.empty()) {
        auto [dis, u] = q.top(); 
        q.pop();
        if (st[u]) {
            continue;
        }
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
