二分图的匹配为满足任意两条边不以同一个顶点为端点的二分图的子图，二分图的最大匹配即为所有匹配中包含边数最多的匹配。匈牙利算法用于求二分图的最大匹配数。将二分图的点集看成左右两半， $n1、n2$ 分别为左、右侧点集数， $m$ 为边数， $cnt$ 表示最大匹配数， $match_i$ 表示右侧 $i$ 点与左侧所匹配的点， $vis_v$ 表示在为点 $i$ 找匹配点的过程中，右侧点 $v$ 的匹配点是否尝试过找**新**点匹配（无匹配点则找点，有匹配点则换点）。

$dfs(u)$：尝试为左侧 $u$ 点找新点匹配，找到返回 $true$，否则返回 $false$，时间复杂度： $O(n1+n2+m)$。

$Hungarian()$：返回二分图的最大匹配数，时间复杂度： $O((n1+n2)m)$。

```c++
auto Hungarian = [&](const vector<vector<int>> &adj, int n1, int n2)->int {
    int cnt = 0;
    vector<int> match(n2 + 1);
    for (int i = 1; i <= n1; i++) {
        vector<bool> vis(n2 + 1);
        auto dfs = [&](auto self, int u)->bool {
            for (int v : adj[u]) {
                if (!vis[v]) {
                    vis[v] = true;
                    if (!match[v] || self(self, match[v])) {
                        match[v] = u;
                        return true;
                    }
                }
            }
            return false;
        };
        cnt += dfs(dfs, i);
    }
    return cnt;
};
```
