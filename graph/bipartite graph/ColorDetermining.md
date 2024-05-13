二分图为能将顶点分为两个点集且每条边的两端点位于不同点集中的图，染色法常用于判定二分图。 $n$ 为点数，颜色 $1、2$ 分别表示两个点集， $color_i$ 表示点 $i$ 所染的颜色，染色过程中若出现相邻两点颜色相同，则不是二分图，反之则是。

$ColorDetermining()$：判定二分图，时间复杂度： $O(n)$。

```c++
auto ColorDetermining = [&](const vector<vector<int>> &adj)->bool {
    int n = adj.size() - 1;
    vector<int> color(n + 1);
    color[0] = 1;
    auto dfs = [&](auto self, int u, int fa)->bool {
        color[u] = 3 ^ color[fa];
        for (int v : adj[u]) {
            if (v != fa) {
                if (!color[v]) {
                    if (!self(self, v, u)) return false;
                }
                else if ((color[u] ^ color[v]) != 3) return false;
            }
        }
        return true;
    };
    for (int i = 1; i <= n; i++)
        if (!color[i]) {
            if (!dfs(dfs, i, 0)) return false;
        }
    return true;
};
```
