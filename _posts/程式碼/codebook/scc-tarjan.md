title: '[codebook] SCC - Tarjan'
date: 2011-10-30 14:39:00
tags:
- SCC
- Strong Connected Component
- 強連通分量
- C++
categories:
- 程式碼
- codebook
---

``` cpp
/*
Strong Connected Component
Tarjan's algorithm
算法描述：
使用 dfn 紀錄 DFS 順序
stack 紀錄點
如果能夠走回較小的 dfn
代表存在強連通分量
於是退棧將強連通點都取出
反覆操作
便可求得所有強連通分量
用相鄰矩陣複雜度為 O(V^2)，若是相鄰列表則為 O(V+E)
*/
```

Pseudo Code:

```
graph G = ( V, E )
create S as stack

Procedure DFS( integer u )

	Index = Index + 1
	dfn [ u ] = low [ u ] = Index
	push u into S

	for each ( u, v ) in E
		if v is not visited then
			tarjan( v )
		if u is in S then
			low [ u ] = min ( low [ u ], low [ v ] )
	end for

	if dfn [ u ] is equal to low [ u ] then

		do
			tmp = top element in S
			pop element in S
		while tmp is not u

End Procedure

Procedure tarjan()

	for u = 1 to n
		if u is not visited then
			DFS( i )
	end for

End Procedure
```

<!-- more -->

C++ Code:

``` cpp
#include &lt;iostream&gt;
#include &lt;vector&gt;
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#define MAX 100010
using namespace std;

vector &lt;int&gt; edge[MAX];
bool instack[MAX];
int dfn[MAX], low[MAX], dfsid, stk[MAX], sptr;
int contract[MAX];

int tarjan(int u)
{
	dfn[u] = low[u] = ++dfsid;
	instack[u] = true;
	stk[++sptr] = u;
	for (int i = 0; i < edge[u].size(); i++) /*DFS*/
	{
		int v = edge[u][i];
		if(!dfn[v] || instack[v]) /*短碼版本*/
			low[u] = min(low[u], ((dfn[v]) ? low[v] : tarjan(v)));
	}
	if(dfn[u] == low[u]) /*抓到環就開始退棧*/
	{
		do
		{
			instack[stk[sptr]] = false;
			contract[stk[sptr]] = u; /*都是在同一環*/
		} while (sptr >= 0 && stk[sptr--] != u);
	}
	return low[u];
}

void SCC()
{
	memset(dfn, 0, sizeof(dfn));
	memset(instack, 0, sizeof(instack));
	dfsid = 0, sptr = -1;
	for (int i = 1; i <= n; i++) /*跑森林*/
		if (!dfn[i]) tarjan(i);
	return;
}
```


另外，為了避免遞迴過深，利用stack來模擬遞迴。

Code:

``` cpp
#include &lt;iostream&gt;
#include &lt;vector&gt;
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#define MAX 100010
using namespace std;

/*SCC*/
vector &lt;int&gt; edge[MAX];
bool instack[MAX];
int dfn[MAX], low[MAX], dfsid, stk[MAX], sptr;
int contract[MAX];

void SCC()
{
	memset(dfn, 0, sizeof(dfn)); /*初始化*/
	memset(instack, 0, sizeof(instack));
	dfsid = 0, sptr = -1;
	int restk[MAX], reit[MAX], resptr = -1; /*模擬遞迴*/
	int v, u, i;
	for (u = 1; u <= n; u++)
	{
		if (!dfn[u])
		{
Inker:
			restk[++resptr] = u;
			dfn[u] = low[u] = ++dfsid;
			instack[u] = true;
			stk[++sptr] = u;
			for (i = 0; i < edge[u].size(); i++)
			{
				v = edge[u][i];
				if (!dfn[v])
				{
					reit[resptr] = i; /*模擬遞迴往下遞*/
					u = v;
					goto Inker;
				}
Kuo:
				if(instack[v])
					low[u] = min(low[u], low[v]);
			}
			if(dfn[u] == low[u]) /*縮點*/
			{
				do
				{
					instack[stk[sptr]] = false;
					contract[stk[sptr]] = u;
				} while (sptr >= 0 && stk[sptr--] != u);
			}
			v = u; /*模擬遞迴 return 實作*/
			resptr--;
			if (resptr < 0) continue;
			i = reit[resptr];
			u = restk[resptr];
			goto Kuo;
		}
	}
	return;
}
```