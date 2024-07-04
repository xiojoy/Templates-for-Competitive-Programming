```c++
int q;
cin >> q;

auto cmp = [&](int i, int j)->bool {
    return hld.in[i] < hld.in[j];
};
vector<bool> bj(n + 1);
vector<int> dp(n + 1);
vector<vector<int>> adj(n + 1);
while (q--) {
    int k;
    cin >> k;

    vector<int> h(k);
    for (int i = 0; i < k; i++) {
        cin >> h[i];
    }

    vector<int> tobj;
    for (int i : h) {
        tobj.push_back(i);
        bj[i] = true;
    }

    sort(all(h), cmp);
    for (int i = 1; i < k; i++) {
        h.push_back(hld.lca(h[i - 1], h[i]));
    }

    sort(all(h), cmp);
    h.erase(unique(all(h)), h.end());

    for (int i = 1; i < h.size(); i++) {
        int LCA = hld.lca(h[i - 1], h[i]);
        adj[LCA].push_back(h[i]);
    }

    vector<int> todfs;
    auto dfs = [&](auto self, int u)->void {
        todfs.push_back(u);
        for (int v : adj[u]) {
            self(self, v);
        }
    };
    dfs(dfs, h[0]);

    for (int u : tobj) {
        bj[u] = false;
    }
    for (int u : todfs) {
        adj[u].clear();
        dp[u] = 0;
    }
}
```
