二分图为能将顶点分为两个点集且每条边的两端点位于不同点集中的图，染色法常用于判定二分图。 $n、m$ 分别为点数、边数，颜色 $1、2$ 分别表示两个点集， $color_i$ 表示点 $i$ 所染的颜色，染色过程中若出现相邻两点颜色相同，则不是二分图，反之则是。

$ColorDetermining(adj)$：判定二分图，时间复杂度： $O(n + m)$。

```c++
auto ColorDetermining = [&](const vector<vector<int>> &adj)->bool {
    int n = adj.size() - 1;
    vector<int> col(n + 1);
    for (int u = 1; u <= n; u++) {
        if (col[u]) {
            continue;
        }
        col[u] = 1;
        bool flag = true;
        auto dfs = [&](auto self, int u)->void {
            for (int v : adj[u]) {
                if (!col[v]) {
                    col[v] = 3 ^ col[u];
                    self(self, v);
                } else if (col[v] == col[u]) {
                    flag = false;
                }
            }
        };
        dfs(dfs, u);
        if (!flag) {
            return false;
        }
    }
    return true;
};
```
