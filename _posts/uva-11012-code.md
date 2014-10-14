title: '[解題報告][UVa][11012] Cosmic Cabbages'
date: 2012-03-01 02:05:00
tags:
- C++
- Math
categories:
- 程式碼
- UVa
---

題目大意：給你三維中 n 個座標，求這 n 個點中最大的曼哈頓距離？（{% math 2\leq{n}\leq{10 ^ 5} %}，多組測資）

這題我擠了好久都想不出來，沒想到神 Inker Kuo 居然連證明都不用，一個數學算法就這樣丟出來，然後留下其他人幫他證明他是對的，根本就是計算界的愛因斯坦你真是神 WORSHIP <(_ _)>

果然這是現世報吧晚上才戳過學弟最遠點對，結果馬上就遇到這題被戳爆 ....... Orz

看來沒有神的資質去冒充神果然下場都很淒慘，冒出旋轉卡尺的想法卻因為是三維空間而沒有做 (隨機增量的凸包還是會爆炸 ......)

看來我還是太弱了，揮一揮衣袖，我帶不走任何電影票 (拭淚 + 嘴砲)

下有雷

<!-- more -->

曼哈頓距離有很強的性質：

| x | + | y | + | z | = d (定值)

因此我們將絕對值解開可以得到 8 個平面：

+ x + y + z = d
+ x + y - z = d
+ x - y + z = d
- x + y + z = d
+ x - y - z = d
- x + y - z = d
- x - y + z = d
- x - y - z = d

取其法向量，扣除重複，可以得到 4 個不重複的法向量：

(+1, +1, +1)
(+1, +1, -1)
(+1, -1, +1)
(-1, +1, +1)

將所有點投影到法向量上，可以得到一條一維，類似「距離表」的關係，在這四條法向量中，各取最大值和最小值，相減後得到 4 個在該法向量上最大的距離，取 max 即可得最大曼哈頓距離。

踏馬得讓我想一下證明 (這實在太詭譎了) ...... 待補。

Code：

``` cpp
#include <stdio.h>
#include <iostream>
#define N 100010
using namespace std;

struct point
{
	int x, y, z;
}p[N];
int n;

int sol(int a, int b, int c)
{
	int mx, mn, m, i;
	mx = mn = a * p[0].x + b * p[0].y + c * p[0].z;
	for (i = 1; i < n; i++)
	{
		m = a * p[i].x + b * p[i].y + c * p[i].z;
		mx = max(mx, m);
		mn = min(mn, m);
	}
	return mx - mn;
}

int main()
{
	int i, t, ctr;
	for (scanf("%d", &t), ctr = 1; ctr <= t; ctr++)
	{
		scanf("%d", &n);
		for (i = 0; i < n; i++)
			scanf("%d%d%d", &p[i].x, &p[i].y, &p[i].z);
		printf("Case #%d: %d\n", ctr, max(max(sol(1, 1, 1), sol(1, 1, -1)), max(sol(1, -1, 1), sol(-1, 1, 1))));
	}
}
```