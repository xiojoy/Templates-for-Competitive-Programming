树的直径为树上任意两点之间最长的简单路径。记 $n$ 为树的点数，树的直径通常有两种求法：

法一（适用于无负权边）：对任意一点 $x$ 用 $BFS$ 求其到树上所有点的最短距离，其中最远点 $u$ 即为直径的一个端点，再对点 $u$ 用 $BFS$ 求其到树上所有点的最短距离，其中最远点 $v$ 即为直径的另一端点。

$get\\_diameter(adj)$：返回树的直径，时间复杂度： $O(n)$。

```c++
auto get_diameter = [&](const vector<vector<array<int, 2>>> &adj) {
    constexpr int inf = 1e9;
    int n = adj.size() - 1;
    auto BFS = [&](int u) {
        vector<int> d(n + 1, inf);
        queue<int> q;
        d[u] = 0;
        q.push(u);
        while (!q.empty()) {
            int u = q.front();
            q.pop();
            for (auto [v, w] : adj[u]) {
                if (d[v] == inf) {
                    d[v] = d[u] + w;
                    q.push(v);
                }
            }
        }
        return d;
    };
    vector<int> dx = BFS(1);
    int u = max_element(ALL(dx)) - dx.begin();
    vector<int> du = BFS(u);
    int v = max_element(ALL(du)) - du.begin();
    return du[v];
};
```

法二（不限制边权）：以某点为根做树形 $DP$， $dp_{u,0}、dp_{u,1}$ 分别表示在以点 $u$ 为根的子树中，从根 $u$ 到叶子节点的最长和次长距离。

```c++
auto get_diameter = [&](const vector<vector<array<int, 2>>> &adj) {
    vector<array<int, 2>> dp(n + 1);
    auto dfs = [&](auto self, int u, int fa)->void {
        for (auto [v, w] : adj[u]) {
            if (v != fa) {
                self(self, v, u);
                if (dp[v][0] + w > dp[u][0]) {
                    dp[u] = {dp[v][0] + w, dp[u][0]};
                }
                else if (dp[v][0] + w > dp[u][1]) {
                    dp[u][1] = dp[v][0] + w;
                }
            }
        }
    };
    dfs(dfs, 1, 0);
    int d = INT_MIN;
    for (int i = 1; i <= n; i++) {
        d = max(d, dp[i][0] + dp[i][1]);
    }
    return d;
};
```

例题：

[Minimum Maximum Distance](https://codeforces.com/contest/1881/problem/F)

[Gardening Friends](https://codeforces.com/contest/1822/problem/F)
