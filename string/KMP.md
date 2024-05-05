$KMP$ 常用于字符串模式匹配。 $S$  为目标串， $P$ 为模式串， $m$ 为 $S$ 的长度， $n$ 为 $P$ 的长度， $ne_i$ 表示前缀 $i-1$（ $0-index$） 的 $border$（即最长公共真前后缀，仅适用于 $get\\_next()$ 求出的 $ne$ 数组）和当 $P_i$ 匹配失败时应往前继续匹配的下标（两个函数都适用，默认 $-1$ 为终止符， $ne[0]=-1$），匹配过程解释见注释。

$get\\_next(P)$：求 $P$ 的 $ne$ 数组，时间复杂度： $O(n)$​。

$get\\_nextval(P)$：求 $P$ 的 $ne$ 数组， $get\\_next$ 的强化版，模式匹配时使用该函数求出的 $ne$ 数组，时间复杂度： $O(n)$。

$KMP()$：模式匹配过程，求 $P$ 作为 $S$ 的子串出现的所有位置（此处默认可重叠，不可重叠需在 $j = n$ 时将 $j$ 赋值为 $0$），时间复杂度： $O(n+m)$。

```C++
vector<int> get_next(const string &P) {
    int n = P.size();
	vector<int> ne(n + 1, -1);
	int i = 0, j = -1;
	while (i < n) { // 借助ne[i]求ne[i+1]，求ne[n]是为了在整个目标串与模式串成功匹配后应再往前调整求模式串中其他位置的匹配
		if (~j && P[i] != P[j]) j = ne[j]; // 匹配失败且j不为-1时往前调整
		else ne[++i] = ++j; // 匹配成功则有必要再匹配一次
	}
    return ne;
}
vector<int> get_nextval(const string &P) {
    int n = P.size();
	vector<int> ne(n + 1, -1);
	int i = 0, j = -1;
	while (i < n) {
		if (~j && P[i] != P[j]) j = ne[j];
        else if (P[++i] == P[++j]) ne[i] = ne[j]; // 匹配成功且在i和j自增后i与j处字符相等，则显然当j匹配失败时ne[j]匹配必然也失败，则可以直接使ne[i]=ne[j]减少不必要的匹配步骤
		else ne[++i] = ++j; // 匹配成功且在i和j自增后i与j处字符不相等，则有必要再匹配一次
	}
    return ne;
}
void KMP() {
    string S, P;
    auto ne = get_nextval(P);
    int m = S.size(), n = P.size();
    int i = 0, j = 0; // 开始匹配，匹配过程同get_next
    while (i < m) {
        if (~j && (j == n || S[i] != P[j])) j = ne[j];
        else i++, j++;
        if (j == n) cout << i - j + 1 << '\n';
    }
}
```

例题：

[Div3_943_G1](https://codeforces.com/contest/1968/problem/G1)
