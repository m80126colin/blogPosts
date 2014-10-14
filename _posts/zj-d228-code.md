title: '[解題報告][ZeroJudge][d228] kill man'
date: 2011-08-23 16:48:00
tags:
- C++
- Josephus Problem
- Dynamic Programming
categories:
- 程式碼
- ZeroJudge
---

# 大意

約瑟夫問題，有 n 個人，每數 k 人殺一次，問殺的第 m 人為誰？ (人物編號由 1 ~ n)

以下有雷

<!-- more -->

# 解法

數學的遞迴解，程式由 DP 實現。

當作 n 個人編號為 0 ~ n - 1 ，從 0 開始數，我們找到編號第 (k - 1) % n = k' 的人被殺。

剩下 n - 1 人重新編號：

```
k' + 1 -> 0
k' + 2 -> 1
k' + 3 -> 2
...
...
k' - 1 -> n - 2
k' -> 被殺
```

於是人數只剩 n - 1 人，「新」編號從 0 ~ n - 2 。
再重複一次，被殺的人為新編號的第 k'' = (k - 1) % (n - 1) 號，以此類推。
由上規律觀察，我們發現「舊」編號與「新」編號位移了 k % n 。
所以我們可以推出「舊」編號 x 和「新」編號 x' 的關係式為：

```
x = (x' + k % n) % n = (x' + k) % n
```

於是得到遞迴關係式：

```
f[i] = 0 when i == 1
f[i] = (f[i - 1] + k) % i when i > 1
```

Index代表剩下幾人。
以上是標準的約瑟夫問題 (Josephus Problem)

現在將約瑟夫變形
殺第 m 個人的編號為何？
殺第 1 人時有 n 個人，殺第 2 人時有 n - 1 個人，因此殺第 m 人時有 n + 1 - m 個人，
此時被殺的第 m 人新編號為 (k - 1) % i ， i = n + 1 - m 意即此時人數。

於是遞迴關係式調整為：

```
f[i] = (k - 1) % i when i == n + 1 - m
f[i] = (f[i - 1] + k) % i when i <= n
```

同樣，Index代表剩下幾人。
算完後的編號是 0 ~ n - 1 之下，調整只要加 1 就可。

# 參考資料

[約瑟夫問題的數學方法](http://epic.yo2.cn/articles/post118.html)

# 參考程式碼

``` cpp
#include <stdio.h>

int n, k, m;

int ans() {
	int i, ans = (k - 1) % (n + 1 - m);
	for (i = m - 1; i; i--) ans = (ans + k) % (n + 1 - i);
	return ans + 1;
}

int main() {
	int t, tc;
	for (scanf("%d", &t), tc = 1; tc <= t; tc++) {
		scanf("%d%d%d", &n, &k, &m);
		printf("Case %d: %d\n", tc, ans());
	}
}
```