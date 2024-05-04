$KMP$ 常用于字符串模式匹配。 $S、P$ 分别为目标串、模式串， 长度分别为 $S、n$， $ne_i$ 表示前缀 $P_{0...i-1}$ 的最长公共真前后缀的长度和当 $P_i$ 匹配失败时应往前继续匹配的下标（默认 $-1$ 为终止符， $ne[0]=-1$），匹配过程解释见注释。

$get\\_next(P)$：求 $P$ 的 $ne$ 数组，时间复杂度： $O(n)$。

$KMP()$：模式匹配过程，求 $P$ 作为 $S$ 的子串出现的所有位置（此处默认可重叠，不可重叠需在 $j = n$ 时将 $j$ 赋值为 $0$），时间复杂度： $O(n+m)$。

```en
vector<int> get_next(const string &P) {
    int n = P.size();
	vector<int> ne(n + 1, -1);
	int i = 0, j = -1;
	while (i < n) {// 借助ne[i]求ne[i+1]，求ne[n]是为了在整个模板串与模式串成功匹配后应再往前调整求模式串中其他位置的匹配
		if (~j && P[i] != P[j]) j = ne[j]; // 匹配失败且j不为-1时往前调整
		else if (P[++i] == P[++j]) ne[i] = ne[j]; // 匹配成功且在i和j自增后i与j处字符相等，则显然当j匹配失败时ne[j]匹配必然也失败，则可以直接使ne[i]=ne[j]减少不必要的匹配步骤
		else ne[i] = j; // 匹配成功且在i和j自增后i与j处字符不相等，则有必要再匹配一次
	}
    return ne; // 返回ne数组
}

void KMP() {
    string S, P;
    auto ne = get_next(P);
    int m = S.size(), n = P.size();
    int i = 0, j = 0; // 开始匹配，过程同求ne数组的过程
    while (i < m) {
        if (~j && (j == n || S[i] != P[j])) j = ne[j];
        else i++, j++;
        if (j == n) cout << i - j + 1 << '\n';
    }
}
```

