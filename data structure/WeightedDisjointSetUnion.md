$WeighedDisjointSetUnion$ 在 [DisjointSetUnion](https://github.com/xiojoy/Templates-for-Competitive-Programming/blob/main/data%20structure/DisjointSetUnion.md) 的基础上使树边带权，可以维护更多额外的信息，如子节点和父节点的关系等， $d[i]$ 表示点 $i$ 到其所在树的根节点的距离。

以下操作时间复杂度均与 $find$ 相同。

$merge(x, y, w)$：在 $x$ 和 $y$ 之间加一条权为 $w$ 的边。

$dist(x, y)$： $x$ 与 $y$ 所在集合相同时返回两点间的距离，否则返回 $-1$。

未解释的操作释义均与 $DisjointSetUnion$ 中的相同或额外维护了 $d$ 的权值。

```C++
class WeightedDisjointSetUnion {
private:
    vector<int> f, siz;
    vector<int> d;
public:
    WeightedDisjointSetUnion() {}
    WeightedDisjointSetUnion(int n) {
        init(n);
    }
    
    void init(int n) {
        f.resize(n);
        d.resize(n);
        iota(f.begin(), f.end(), 0);
        siz.assign(n, 1);
    }
    
    int find(int x) {
        if (x != f[x]) {
            int fx = f[x];
            f[x] = find(f[x]);
            d[x] += d[fx];
        }
        return f[x];
    }
    
    bool same(int x, int y) {
        return find(x) == find(y);
    }
    
    bool merge(int x, int y, int w) {
        int fx = find(x);
        int fy = find(y);
        if (x == y) {
            return false;
        }
        d[fy] = d[x] + w - d[y];
        siz[fx] += siz[fy];
        f[fy] = fx;
        return true;
    }

    int size(int x) {
        return siz[find(x)];
    }

    int dist(int x, int y) {
        return same(x, y) ? d[y] - d[x] : -114514;
    }
};
```

例题：

[CEOI1999](https://www.luogu.com.cn/problem/P5937)

