```c++
struct Info {
    int len = 0;
    int link = 0;
    array<int, 26> next{};
};

struct SuffixAutomaton {
    int last = 0;
    vector<Info> t;

    SuffixAutomaton() {
        init();
    }

    void init() {
        t.clear();
        newNode();
        t.back().link = -1;
    }
    int newNode() {
        t.emplace_back();
        return t.size() - 1;
    }
    void extend(int x) {
        x -= 'a';
        int cur = newNode();
        t[cur].len = t[last].len + 1;
        int p = last;
        while (~p && !t[p].next[x]) {
            t[p].next[x] = cur;
            p = t[p].link;
        }
        if (p == -1) {
            t[cur].link = 0;
        } else {
            int q = t[p].next[x];
            if (t[p].len + 1 == t[q].len) {
                t[cur].link = q;
            } else {
                int clo = newNode();
                t[clo].link = t[q].link;
                t[clo].next = t[q].next;
                t[clo].len = t[p].len + 1;
                while (~p && t[p].next[x] == q) {
                    t[p].next[x] = clo;
                    p = t[p].link;
                }
                t[q].link = t[cur].link = clo;
            }
        }
        last = cur;
    }
};
```
