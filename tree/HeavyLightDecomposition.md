树链剖分用于将整棵树剖分为若干条链，使它组合成线性结构，然后用其他的数据结构维护信息。 $n$ 表示节点个数， $cur$ 用于求时间戳， $siz_i$ 表示子树 $i$ 的大小， $top_i$ 表示 点 $i$ 所在重链的顶部节点， $dep_i$ 表示点 $i$ 的深度， $parent_i$ 表示点 $i$ 的父节点， $in_i$ 表示进入点 $i$ 的时间戳， $out_i$ 表示离开点 $i$ 的时间戳， $seq_i$ 表示进入时间戳为 $i$​​ 时对应的节点。

$init(n)$ ：初始化，时间复杂度： $O(n)$。

$add\\_edge(u, v)$ ：添加无向边，时间复杂度： $O(1)$。

$work(root=0)$ ：进行树链剖分，时间复杂度： $O(n)$。

$dfs1(u)$ ：求 $siz、parent、dep$，将点 $u$ 的重儿子置为 $adj_{u,\0}$ 并删除点 $u$ 向父亲的边，时间复杂度： $O(n)$。

$dfs2(u)$ ：求 $in、seq、top、out$，时间复杂度： $O(n)$。

$lca(u,v)$ ：求点 $u$ 和 $v$ 的最近公共祖先，时间复杂度： $O(logn)$。

$dist(u,v)$ ：求点 $u$ 和 $v$ 的距离，时间复杂度： $O(logn)$​。

$jump(u,k)$ ：求点 $u$ 的第 $k$ 级祖先，时间复杂度： $O(logn)$。

$is\\_ancestor(u,v)$ ：判断 $u$ 是否为 $v$ 的祖先或本身，时间复杂度： $O(1)$。

$rooted\\_parent(u,v)$ ：求以点 $u$ 为根节点时 $v$ 的父亲，时间复杂度： $O(logn)$​。

$rooted\\_size(u,v)$ ：求以点 $u$ 为根节点时 $v$ 子树的大小，时间复杂度： $O(logn)$​。

$rooted\\_size(a,b,c)$ ：求以点 $a$ 为根节点时 $b$ 和 $c$ 的最近公共祖先，时间复杂度： $O(logn)$。

```c++
struct HLD {
    int n;
    vector<int> siz, top, dep, parent, in, out, seq;
    vector<vector<int>> adj;
    int cur;
    
    HLD() {}
    HLD(int n) {
        init(n);
    }
    void init(int n) {
        this->n = n - 1;
        siz.resize(n);
        top.resize(n);
        dep.resize(n);
        parent.resize(n);
        in.resize(n);
        out.resize(n);
        seq.resize(n);
        cur = 0;
        adj.assign(n, {});
    }
    void add_edge(int u, int v) {
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    void work(int root = 1) {
        top[root] = root;
        dep[root] = 0;
        parent[root] = 0;
        dfs1(root);
        dfs2(root);
    }
    void dfs1(int u) {
        if (parent[u] != -1) {
            adj[u].erase(find(all(adj[u]), parent[u]));
        }
        
        siz[u] = 1;
        for (auto &v : adj[u]) {
            parent[v] = u;
            dep[v] = dep[u] + 1;
            dfs1(v);
            siz[u] += siz[v];
            if (siz[v] > siz[adj[u][0]]) {
                swap(v, adj[u][0]);
            }
        }
    }
    void dfs2(int u) {
        in[u] = cur++;
        seq[in[u]] = u;
        for (auto v : adj[u]) {
            top[v] = v == adj[u][0] ? top[u] : v;
            dfs2(v);
        }
        out[u] = cur;
    }
    int lca(int u, int v) {
        while (top[u] != top[v]) {
            if (dep[top[u]] > dep[top[v]]) {
                u = parent[top[u]];
            } else {
                v = parent[top[v]];
            }
        }
        return dep[u] < dep[v] ? u : v;
    }
    
    int dist(int u, int v) {
        return dep[u] + dep[v] - 2 * dep[lca(u, v)];
    }
    
    int jump(int u, int k) {
        if (dep[u] < k) {
            return -1;
        }
        
        int d = dep[u] - k;
        
        while (dep[top[u]] > d) {
            u = parent[top[u]];
        }
        
        return seq[in[u] - dep[u] + d];
    }
    
    bool is_ancester(int u, int v) {
        return in[u] <= in[v] && in[v] < out[u];
    }
    
    int rooted_parent(int u, int v) {
        swap(u, v);
        if (u == v) {
            return u;
        }
        if (!is_ancester(u, v)) {
            return parent[u];
        }
        return *--upper_bound(all(adj[u]), v, [&](int x, int y) { return in[x] < in[y]; });
    }
    
    int rooted_size(int u, int v) {
        if (u == v) {
            return n;
        }
        if (!is_ancester(v, u)) {
            return siz[v];
        }
        return n - siz[rooted_parent(u, v)];
    }
    
    int rooted_lca(int a, int b, int c) {
        return lca(a, b) ^ lca(b, c) ^ lca(c, a);
    }
};
```
