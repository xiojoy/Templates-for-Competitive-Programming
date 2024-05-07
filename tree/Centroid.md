树的重心是指树中的一个结点，且将该点删除后，剩余最大连通块的点数的最小。记 $n$ 为树的点数，直接 $dfs$ 求重心即可，时间复杂度： $O(n)$。

```c++
auto get_root = [&]()->int {
    int root = 0, val = INF;
    auto dfs = [&](auto self, int u, int fa)->int {
        int siz = 1, mx = 0;
        for (auto [v, w] : adj[u]) {
            if (v != fa && !vis[v]) {
                int num = self(self, v, u);
                siz += num, mx = max(mx, num);
            }
        }
        mx = max(mx, n - siz);
        if (mx < val) root = u, val = mx;
        return siz;
    };
    dfs(dfs, 1, 0);
    return root;
};
```
