title: '[解題報告][ZeroJudge][d197][UVa][11504] Dominos'
date: 2011-10-30 14:14:00
tags:
- C++
- Stack
- SCC
- Strong Connected Component
categories:
- 程式碼
- ZeroJudge
---

下有雷

<!-- more -->

簡單來想，就是要找出有向圖中有多少個頭，但詳細考慮的話，會發現其實有有向環的存在，因此我們要做縮環為點的動作，形成 DAG 圖。

本題利用遞迴實作 SCC 即可得解，但用 stack 實作 SCC 省去記憶體堆疊和函數呼叫的時間，因此除了更快，更省去因為遞迴過深而造成的記憶體爆炸。

Code:

``` cpp
#include &lt;iostream&gt;
#include &lt;vector&gt;
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#include &lt;ctype.h&gt;
#define MAX 100010
using namespace std;

/*SCC*/
vector &lt;int&gt; edge[MAX];
bool instack[MAX];
int dfn[MAX], low[MAX], dfsid, stk[MAX], sptr;
int contract[MAX];
int n, m;

void SCC()
{
	memset(dfn, 0, sizeof(dfn)); /*初始化*/
	memset(instack, 0, sizeof(instack));
	dfsid = sptr = 0;
	int restk[MAX], reit[MAX], resptr = 0; /*模擬遞迴*/
	int v, u, i;
	for (u = 1; u <= n; u++)
	{
		if (!dfn[u])
		{
Inker:
			restk[resptr++] = u;
			dfn[u] = low[u] = ++dfsid;
			instack[u] = true;
			stk[sptr++] = u;
			for (i = 0; i < edge[u].size(); i++)
			{
				v = edge[u][i];
				if (!dfn[v])
				{
					reit[resptr - 1] = i; /*模擬遞迴往下遞*/
					u = v;
					goto Inker;
				}
Kuo:
				if(instack[v])
					low[u] = min(low[u], low[v]);
			}
			if(dfn[u] == low[u]) /*縮點*/
			{
				sptr--;
				do
				{
					instack[stk[sptr]] = false;
					contract[stk[sptr]] = u;
				} while (sptr >= 0 && stk[sptr--] != u);
				sptr++;
			}
			v = u; /*模擬遞迴 return 實作*/
			resptr--;
			if (resptr <= 0) continue;
			i = reit[resptr - 1];
			u = restk[resptr - 1];
			goto Kuo;
		}
	}
	return;
}

int indeg[MAX], ans;

void sol() /*計算多少點 indegree 為零*/
{
	int i, u, v;
	memset(indeg, 0, sizeof(indeg));
	ans = 0;
	for (u = 1; u <= n; u++)
	{
		for (i = 0; i < edge[u].size(); i++)
		{
			v = edge[u][i];
			if (contract[u] != v) /*縮起來不是縮點就戳*/
				indeg[v]++;
			if (contract[u] != contract[v]) /*縮起來不同點就戳*/
				indeg[contract[v]]++;
		}
	}
	for (u = 1; u <= n; u++)
		if (!indeg[u])
			ans++;
	return;
}

void getd(int &d) /*輸入優化*/
{
	char ch;
	while (!isdigit(ch = getchar())) ;
	d = 0;
	do
	{
		d = d * 10 + ch - '0';
	} while (isdigit(ch = getchar()));
	return;
}

int main()
{
	int t, i, a, b;
	for (getd(t); t; t--)
	{
		getd(n), getd(m);
		for (i = 1; i <= n; i++) edge[i].clear();
		for (i = 0; i < m; i++)
		{
			getd(a), getd(b);
			edge[a].push_back(b);
		}
		SCC();
		sol();
		printf("%d\n", ans);
	}
}
```