拓扑排序用于求图的拓扑序，若不存在拓扑序则说明有环。 $n、m$ 分别为图的点数和边数， $top$数组用于存储拓扑序。若要求所有拓扑序中字典序最小的，则将队列改成小根堆。

时间复杂度：队列： $O(n+m)$，小根堆： $O(nlogn+m)$。

```c++
auto topsort = [&]()->vector<int> {
    queue<int> q;
    for (int i = 1; i <= n; i++) if (!id[i]) q.push(i);
    vector<int> top;
    while (!q.empty()) {
        int u = q.front();
        q.pop();
        top.push_back(u);
        for (int v : adj[u]) if (!--id[v]) q.push(v);
    }
    return top;
};
```
