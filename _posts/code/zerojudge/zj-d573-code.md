title: '[解題報告][ZeroJudge][d573] CRC騎士團'
date: 2010-07-09 21:14:16
tags:
- 程式碼
- C++
categories:
- 程式碼
- ZeroJudge
---

![](/blog/img/20100709-211416-1.jpg)

<!-- more -->

``` cpp
/*
Problem: d573
Language: C++
Result: AC (102ms, 863KB) on ZeroJudge
Author: m80126colin at 2010-07-09 21:13:04
Solution: Straight Forward
*/
#include <iostream>
using namespace std;

int n,i,j,k,x[100001];

int main() {
	while (~scanf("%d",&n)) {
		for (;n;n--) for (scanf("%d%d",&i,&j);j;j--) scanf("%d",&k),x[k]=i;
		scanf("%d",&k);
		printf("%d\n",x[k]);
	}
}
```