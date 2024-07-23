树上启发式合并通常用于解决与子树相关的数量问题。

树上启发式合并的一般过程（按需要与数据结构配合使用）：

1.  先遍历 $u$ 的轻儿子，并计算答案，但 **不保留**遍历后它对 $cnt$ 数组的影响；
2.  遍历它的重儿子，**保留**它对 $cnt$ 数组的影响；
3.  再次遍历 $u$ 的轻儿子的子树结点，加入这些结点的贡献，以得到 $u$ 的答案。

根节点到树上任意节点的轻边数不超过 $\log n$ 条，所以一个节点的被遍历次数为 $\log n+1$。

假设计算答案过程 $O(1)$，则总时间复杂度： $O(nlogn)$。

$get\\_hson()$：求每个节点的重儿子，时间复杂度： $O(n)$。

```c++
vector<int> hson(n + 1), siz(n + 1);
auto get_hson = [&](auto self, int u, int fa)->void {
    siz[u] = 1;
    for (int v : adj[u]) {
        if (v != fa) {
            self(self, v, u);
            siz[u] += siz[v];
            if (siz[v] > siz[hson[u]]) {
                hson[u] = v;
            }
        }
    }
};
get_hson(get_hson, 1, 0);

auto add = [&](auto self, int u, int fa)->void {
    for (int v : adj[u]) {
        if (v != fa) {
            self(self, v, u);
        }
    }
};
auto del = [&](auto self, int u, int fa)->void {
    for (int v : adj[u]) {
        if (v != fa) {
            self(self, v, u);
        }
    }        
};

int ans = 0;
auto dfs = [&](auto self, int u, int fa, bool keep)->void {
    for (int v : adj[u]) {
        if (v != fa && v != hson[u]) {
            self(self, v, u, false);
        }
    }
    if (hson[u]) {
        self(self, hson[u], u, true);
    }

    // add u

    for (int v : adj[u]) {
        if (v != fa && v != hson[u]) {
            add(add, v, u);
        }
    }

    // 计算子树 u

    if (!keep) {
        // del u

        for (int v : adj[u]) {
            if (v != fa) {
                del(del, v, u);
            }
        }            
    }
};
dfs(dfs, 1, 0, true);
```
