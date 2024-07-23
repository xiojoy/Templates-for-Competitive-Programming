
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
