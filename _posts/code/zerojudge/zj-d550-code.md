title: '[解題報告][ZeroJudge][d550] 物件排序'
date: 2010-07-09 14:50:13
tags:
- 程式碼
- C++
categories:
- 程式碼
- ZeroJudge
---

{% owl-img img/20100709-145013-1.jpg %}

<span style="color: red;">下有捏請注意</span>

<!-- more -->

# 原始版

``` cpp
/*
Problem: d550
Language: C++
Result: AC (375ms, 5327KB) on ZeroJudge
Author: m80126colin at 2010-07-09 13:40:13
Solution: Straight Forward
*/
#include <iostream>
#include <algorithm>
using namespace std;

int n,m,s[10000][200],h[10000];

bool cmp(int x,int y) {
	int i;
	for (i=0;i<m;i++) {
		if (s[x][i]!=s[y][i]) return s[x][i]<s[y][i];
	}
	return 1;
}

int main() {
	int i,j;
	while (~scanf("%d%d",&n,&m)) {
		for (i=0;i<n;i++) {
			h[i]=i;
			for (j=0;j<m;j++) scanf("%d",&s[i][j]);
		}
		sort(h,h+n,cmp);
		for (i=0;i<n;i++) {
			for (j=0;j<m;j++) printf("%d ",s[h[i]][j]);
			printf("\n");
		}
	}
}
```

# 短碼版

``` cpp
/*
Problem: d550 (short version)
Language: C++
Result: AC (395ms, 5355KB) on ZeroJudge
Author: m80126colin at 2010-07-09 14:39:35
Solution: Straight Forward
*/
#include <iostream>
int n,m,s[10001][201],h[10001],i,j,z;
bool cmp(int x,int y){for (z=m;z&&s[x][z]==s[y][z];z--) ;return s[x][z]>s[y][z];}
int a(){for (i=n,j=--m;i;j?j--:(j=m,h[i]=i--))scanf("%d",&s[i][j]);for (std::sort(h+1,h+n+1,cmp),i=n,j=m;i;j?j--:(i--,j=m,putchar(10)))printf("%d ",s[h[i]][j]);return 1;}
int main(){~scanf("%d%d",&n,&m)&&a()&& main();}
```