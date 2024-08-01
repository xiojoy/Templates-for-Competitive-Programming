线段树（单点修改、单点操作、区间查询）

```C++
struct Info {
    int x = 0;

    void apply(int t) {
        x += t;
    }
};
Info operator+(const Info &a, const Info &b) {
    return {max(a.x, b.x)};
}

struct SegmentTree {
    int n;
    vector<Info> info;

    #define lc(p) p << 1
    #define rc(p) p << 1 | 1

    SegmentTree() : info(1) {}
    template <class T>
    SegmentTree(const vector<T> &init_) : info(1) {
        init(init_);
    }

    template <class T>
    void init(const vector<T> &init_) {
        auto build = [&](auto self, int p, int l, int r) -> void {
            if (l == r) {
                info[p] = {init_[l]};
            } else {
                int m = l + r >> 1;
                self(self, lc(p), l, m);
                self(self, rc(p), m + 1, r);
                pull(p);
            }
        };
        n = init_.size() - 1;
        info.assign(4 << __lg(n), Info());
        build(build, 1, 1, n);
    }
    void pull(int p) {
        info[p] = info[lc(p)] + info[rc(p)];
    }
    void modify(int p, int l, int r, int x, const Info &v) {
        if (l == r) {
            info[p] = v;
            return;
        }
        int m = l + r >> 1;
        if (x <= m) {
            modify(lc(p), l, m, x, v);
        } else {
            modify(rc(p), m + 1, r, x, v);
        }
        pull(p);
    }
    void modify(int x, const Info &v) {
        modify(1, 1, n, x, v);
    }
    Info rangeQuery(int p, int l, int r, int x, int y) {
        if (l > y || r < x) {
            return Info();
        }
        if (l >= x && r <= y) {
            return info[p];
        }
        int m = l + r >> 1;
        return rangeQuery(lc(p), l, m, x, y) + rangeQuery(rc(p), m + 1, r, x, y);
    }
    Info rangeQuery(int x, int y) {
        return rangeQuery(1, 1, n, x, y);
    }
    template<class T>
    void apply(int p, int l, int r, int x, const T &v) {
        if (l == r) {
            info[p].apply(v);
            return;
        }
        int m = l + r >> 1;
        if (x <= m) {
            apply(lc(p), l, m, x, v);
        } else {
            apply(rc(p), m + 1, r, x, v);
        }
        pull(p);
    }
    template<class T>
    void apply(int x, const T &v) {
        apply(1, 1, n, x, v);
    }
};
```

例题：

[P1198](https://www.luogu.com.cn/problem/P1198)

[AcW245](https://www.acwing.com/problem/content/246)
