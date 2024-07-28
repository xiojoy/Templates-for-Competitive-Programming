线段树（单点修改、单调操作、区间查询）

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



懒标记线段树（单点修改、区间操作、区间查询）

```C++
struct Tag {
    int x = 0;

    void apply(const Tag &t) {
        x += t.x;
    }
};

struct Info {
    int x = 0;

    void apply(const Tag &t) {
        x += t.x;
    }
};
Info operator+(const Info &a, const Info &b) {
    return {max(a.x, b.x)};
}

struct LazySegmentTree {
    int n;
    vector<Tag> tag;
    vector<Info> info;

    #define lc(p) p << 1
    #define rc(p) p << 1 | 1

    LazySegmentTree() : info(1), tag(1) {}
    template <class T>
    LazySegmentTree(const vector<T> &init_) : info(1), tag(1) {
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
        tag.assign(4 << __lg(n), Tag());
        info.assign(4 << __lg(n), Info());
        build(build, 1, 1, n);
    }
    void pull(int p) {
        info[p] = info[lc(p)] + info[rc(p)];
    }
    void apply(int p, const Tag &v) {
        info[p].apply(v);
        tag[p].apply(v);
    }
    void push(int p) {
        if (tag[p].x) {
            apply(lc(p), tag[p]);
            apply(rc(p), tag[p]);
            tag[p] = Tag();
        }
    }
    void modify(int p, int l, int r, int x, const Info &v) {
        if (l == r) {
            info[p] = v;
            return;
        }
        int m = l + r >> 1;
        push(p);
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
        push(p);
        int m = l + r >> 1;
        return rangeQuery(lc(p), l, m, x, y) + rangeQuery(rc(p), m + 1, r, x, y);
    }
    Info rangeQuery(int x, int y) {
        return rangeQuery(1, 1, n, x, y);
    }
    void rangeApply(int p, int l, int r, int x, int y, const Tag &v) {
        if (l > y || r < x) {
            return;
        }
        if (l >= x && r <= y) {
            apply(p, v);
            return;
        }
        push(p);
        int m = l + r >> 1;
        rangeApply(lc(p), l, m, x, y, v);
        rangeApply(rc(p), m + 1, r, x, y, v);
        pull(p);
    }
    void rangeApply(int x, int y, const Tag &v) {
        rangeApply(1, 1, n, x, y, v);
    }
};
```



动态懒标记线段树（动态开点、单点修改、区间操作、区间查询）

```C++
struct Tag {
    int x = 0;

    void apply(const Tag &t) {
        x += t.x;
    }    
};

struct Info {
    int x = 0;
    int len = 0;
    int lc = 0;
    int rc = 0;

    void apply(const Tag &t) {
        x += t.x;
    }
};
Info operator+(const Info &a, const Info &b) {
    return {max(a.x, b.x), a.len + b.len};
}

struct DynamicLazySegmentTree {
    int n;
    vector<Tag> tag;
    vector<Info> info;

    #define lc(p) info[p].lc
    #define rc(p) info[p].rc

    DynamicLazySegmentTree() : info(1), tag(1)  {}
    template <class T>
    DynamicLazySegmentTree(const vector<T> &init_) : info(1), tag(1) {
        init(init_);
    }

    template <class T>
    void init(const vector<T> &init_) {
        auto build = [&](auto self, int p, int l, int r) -> void {
            if (l == r) {
                info[p] = {init_[l], 1};
            } else {
                make(p);
                int m = l + r >> 1;
                self(self, lc(p), l, m);
                self(self, rc(p), m + 1, r);
                pull(p);
            }
        };
        n = init_.size() - 1;
        tag.push_back(Tag());
        info.push_back(Info());
        build(build, 1, 1, n);    
    }
    void make(int p) {
        make_lc(p);
        make_rc(p);
    }
    void make_lc(int p) {
        if (!lc(p)) {
            lc(p) = info.size();
            tag.push_back(Tag());
            info.push_back(Info());
        }
    }
    void make_rc(int p) {
        if (!rc(p)) {
            rc(p) = info.size();
            tag.push_back(Tag());
            info.push_back(Info());
        }
    }
    void pull(int p) {
        int lc = info[p].lc, rc = info[p].rc;
        info[p] = info[lc(p)] + info[rc(p)];
        info[p].lc = lc;
        info[p].rc = rc;
    }
    void apply(int p, const Tag &v) {
        info[p].apply(v);
        tag[p].apply(v);
    }
    void push(int p) {
        if (tag[p].x) {
            make(p);
            apply(lc(p), tag[p]);
            apply(rc(p), tag[p]);
            tag[p] = Tag();
        }
    }
    void modify(int p, int l, int r, int x, const Info &v) {
        if (l == r) {
            info[p] = v;
            return;
        }
        int m = l + r >> 1;
        push(p);
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
        push(p);
        int m = l + r >> 1;
        return rangeQuery(lc(p), l, m, x, y) + rangeQuery(rc(p), m + 1, r, x, y);
    }
    Info rangeQuery(int x, int y) {
        return rangeQuery(1, 1, n, x, y);
    }
    void rangeApply(int p, int l, int r, int x, int y, const Tag &v) {
        if (l > y || r < x) {
            return;
        }
        if (l >= x && r <= y) {
            apply(p, v);
            return;
        }
        push(p);
        int m = l + r >> 1;
        rangeApply(lc(p), l, m, x, y, v);
        rangeApply(rc(p), m + 1, r, x, y, v);
        pull(p);
    }
    void rangeApply(int x, int y, const Tag &v) {
        rangeApply(1, 1, n, x, y, v);
    }
};
```

