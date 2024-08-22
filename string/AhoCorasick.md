```c++
struct Info {
    int link = 0;
    array<int, 26> next{};
};

struct AhoCorasick { 
    vector<Info> t;

    AhoCorasick() {
        init();
    }

    void init() {
        t.clear();
        newNode();
    }
    int newNode() {
        t.emplace_back();
        return t.size() - 1;
    }
    void insert(const string &s) {
        int p = 0;
        for (char c : s) {
            int u = c - 'a';
            if (!t[p].next[u]) {
                t[p].next[u] = newNode();
            }
            p = t[p].next[u];
        }
    };
    void work() {
        queue<int> q;
        for (int u = 0; u < 26; u++) {
            if (t[0].next[u]) {
                q.push(t[0].next[u]);
            }
        }
        while (!q.empty()) {
            int p = q.front();
            q.pop();
            for (int u = 0; u < 26; u++) {
                if (t[p].next[u]) {
                    t[t[p].next[u]].link = t[t[p].link].next[u];
                    q.push(t[p].next[u]);
                } else {
                    t[p].next[u] = t[t[p].link].next[u];
                }
            }
        }
    }
};
```

例题：

[Edu_134_E](https://codeforces.com/contest/1721/problem/E)

[P2292](https://www.luogu.com.cn/problem/P2292)

[Edu_71_G](https://codeforces.com/contest/1207/problem/G)
