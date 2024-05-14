树的重心是指树中的一个结点，且将该点删除后，剩余最大连通块的点数的最小。 $n$ 为树的点数， $siz$ 为以 $u$ 为根的子树大小， $mx$ 为将点 $u$ 删除后剩余最大连通块的点数， $cent$ 为树的重心， $val$ 为将重心删除后剩余最大连通块的点数。

$get\\_centroid()$：返回树的重心，时间复杂度： $O(n)$。

```c++
auto get_centroid = [&](const vector<vector<int>> &adj)->int {
    int cent = 0, val = INF, n = adj.size() - 1;
    auto dfs = [&](auto self, int u, int fa)->int {
        int siz = 1, mx = 0;
        for (int v : adj[u]) {
            if (v != fa) {
                int num = self(self, v, u);
                siz += num, mx = max(mx, num);
            }
        }
        mx = max(mx, n - siz);
        if (mx <= val) cent = u, val = mx;
        return siz;
    };
    dfs(dfs, 1, 0);
    return cent;
};
```
