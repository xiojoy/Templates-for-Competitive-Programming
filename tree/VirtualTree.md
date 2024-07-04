当题目有多次询问且每次询问可以用树形 $dp$ 处理且多次询问的必须被 $dp$ 的点的数量与 $n$ 同级时，可以在每次询问中建立只包含当前询问所必须被 $dp$ 的点的虚树， $q$ 为询问数， $cmp$ 用于将每次询问的标记点按 $dfs$ 序排序， $bj_i$ 表示点 $i$ 在是否被标记， $dp_i$ 含义视具体题目而定， $adj$ 常规存边， $k$ 为当前询问标记点的数量， $h$ 用于存储标记点和必须被 $dp$ 的点的数量， $tobj$ 和 $todfs$ 用于还原询问前的状态。

每次询问中建虚树的一般过程（常与 $ut$ 或 [HeavyLightDecomposition](https://github.com/xiojoy/Templates-for-Competitive-Programming/blob/main/tree/HeavyLightDecomposition.md) 配合使用）：

1. 先对所有标记点按 $dfs$ 序排序；
2. 将相邻（下标差值的绝对值为 $1$ ）的标记点的在原树中的 $lca$ 添加至 $h$ 中； 
3. 此时 $h$ 中可能有重复的点，所以需要排序去重；
4. 建虚树，在 $h$ 中下标不为 $0$ 的每个点 $a$ 与其前一个点的 $lca$ 上往点 $a$ 连一条有向边，边权为原树中两点的距离。

接着可以正常进行 $dp$ 操作，最后还原。

总时间复杂度： $O(nlogn)$ 。

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

(Div3_805_G2)[https://codeforces.com/contest/1702/problem/G2]
