$DisjointSetUnion$ 是一种树型的数据结构，用于处理一些不相交集合的合并及查询问题， $f[i]$ 表示点 $i$ 所在树的根节点编号， $siz[i]$ 表示点 $i$ 所在树的大小。

$init(n)$：初始化，每个点本身为一棵树和根节点，时间复杂度： $O(n)$。

$find(x)$：查询 $x$ 所在子树的根节点并路径压缩，时间复杂度：对于某点，首次 $O(logn)$，之后 $O(1)$。

以下操作时间复杂度均与 $find$ 相同。

$same(x, y)$：查询 $x$ 和 $y$ 所属集合是否相同。

$merge(x,y)$：将 $y$ 所在的集合合并至 $x$ 所在的集合中，成功合并返回 $true$，失败（ $x$ 和 $y$ 已在一个集合中）返回 $false$。

$size(x)$：查询 $x$ 所在集合的大小。

```C++
struct DisjointSetUnion {
    vector<int> f, siz;

    DisjointSetUnion() {}
    DisjointSetUnion(int n) {
        init(n); 
    }

    void init(int n) { 
        f.resize(n);
        iota(ALL(f), 1);
        siz.assign(n, 1); 
    }
    int find(int x) {
        while (x != f[x]) {
            x = f[x] = f[f[x]];
        }
        return x;
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
        siz[x] += siz[y];
        f[y] = x;
        return true;
    }
    int size(int x) { 
        return siz[find(x)]; 
    }
};
```

例题：

[ABC_049_D](https://atcoder.jp/contests/abc049/tasks/arc065_b)
