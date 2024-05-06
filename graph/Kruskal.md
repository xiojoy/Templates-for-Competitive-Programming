$Kruskal$ 用于求连通图的最小生成树。 $n、m$ 分别为图的点数和边数， $DSU$ 为并查集， $cnt$ 为已选择的边数， $sum$ 为最小生成树的权值。

$Kruskal()$： 图非连通时返回 $-1$，否则返回最小生成树的权值，时间复杂度： $O(mlogm)$。

```c++
auto Kruskal = [&]()->i64 {
    DisjointSetUnion DSU(n + 1);
    sort(all(E));
    i64 sum = 0, cnt = 0;
    for (auto [w, u, v] : E) {
        if (cnt == n - 1) break;
        if (DSU.merge(u, v)) sum += w, cnt++;
    }
    return cnt == n - 1 ? sum : -1;
};
```

例题：

[ABC_352_E](https://atcoder.jp/contests/abc352/tasks/abc352_e)
