$2-SAT$ 是指如后文一类的问题，给出 $n$ 个集合，每个集合有 $2$ 个元素，已知 $m$ 个矛盾条件 $(a,\ b)$，问是否存在某种选法使得能从每个集合中选出一个元素且两两不矛盾。

考虑如下一种情况，有两个集合分别为 $\\{a,\ b\\}、 \\{c,\ d\\}$，存在一个矛盾条件 $(a,\ c)$，则我们可以推出选 $a$ 时一定选 $d$，选 $c$ 时一定选 $b$，则可以考虑对此建图加边，连两条有向边从 $a$ 到 $d$ 和 从 $c$ 到 $b$。有多集合和多矛盾时也可以类似建图。 

判断是否存在选法，只需要看，同一集合的两元素是否在同一个强连通分量，是则不存在选法，否则存在，此处用 [tarjan](https://github.com/xiojoy/Templates-for-Competitive-Programming/blob/main/graph/strongly%20connected%20component/Tarjan.md) 求强连通分量。若要输出具体方案，则可看同一集合的两元素谁所在的强连通分量的拓扑序，谁的更靠后则选谁，由于 $tarjan$ 后求出的强连通分量编号相当于 $dag$ 的反拓扑序，则可比较谁的强连通分量编号更靠前，谁的更靠前则选谁。

建图时，把第 $i$ 个集合的两元素分别记为点 $i$ 和 点 $i+n$。 $u、v$ 分别为集合编号， $x、y$ 分别表示第 $u$ 个集合的第 $x$ 个元素和第 $v$ 个集合的第 $y$ 个元素（存在矛盾），则对该矛盾的建图代码如下：

```c++
adj[u + x * n].push_back(v + !y * n);
adj[v + y * n].push_back(u + !x * n); 
```

$TwoSat(scc,\ n)$：返回 $ans$ 数组， $ans_i$ 表示是否选择点 $i$（未选择 $i$ 则选择 $i+n$）， $ans_0$ 为 $1$ 时表示不存在选法。时间复杂度： $O(n+m)$。

```c++
auto TwoSat = [&](const vector<int> &scc, int n)->vector<bool> {
    vector<bool> ans(n + 1);
    for (int i = 1; i <= n; i++) {
        if (scc[i] == scc[i + n]) {
            ans[0] = 1;
            break;
        }
        else ans[i] = scc[i] < scc[i + n];
    }
    return ans;
};
```

例题：

[HDU3062](http://acm.hdu.edu.cn/showproblem.php?pid=3062)

[P4782](https://www.luogu.com.cn/problem/P4782)

[Div4_944_H](https://codeforces.com/contest/1971/problem/H)
