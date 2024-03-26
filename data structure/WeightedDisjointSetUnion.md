$WeighedDisjointSetUnion$​ 在 [DisjointSetUnion](https://github.com/xiojoy/Templates-for-Competitive-Programming/blob/main/data%20structure/DisjointSetUnion.md) 的基础上使树边带权，可以维护更多额外的信息，如子节点和父节点的关系等， $d[i]$ 表示点 $i$ 到其所在树的根节点的距离。

$dist(x, y)$： $x$ 与 $y$ 所在集合相同时返回两点间的距离，否则返回 $-1$，时间复杂度与 $find$ 相同。

未解释的操作释义均与 $DisjointSetUnion$ 中的相同或额外维护了 $d$ 的权值。

```C++
class WeightedDisjointSetUnion {
private:
    vector<int> f, siz;
    vector<int> d;
public:
    DisjointSetUnion() {}
    DisjointSetUnion(int n) {
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
    
    bool merge(int x, int y) {
        x = find(x);
        y = find(y);
        if (x == y) {
            return false;
        }
        d[y] += siz[x];
        siz[x] += siz[y];
        f[y] = x;
        return true;
    }

    int size(int x) {
        return siz[find(x)];
    }

    int dist(int x, int y) {
        return same(x, y) ? abs(d[x] - d[y]) : -1;
    }
};
```
