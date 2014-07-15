title: '[解題報告][ZeroJudge][d587] 參貳壹真好吃'
date: 2010-07-12 19:59:13
tags:
- 程式碼
- C++
categories:
- 程式碼
- ZeroJudge
---

![](/blog/img/20100712-195913-1.jpg)

<!-- more -->

``` cpp
/*
Problem: d587
Language: C++
Result: AC (6ms, 688KB) on ZeroJudge
Author: m80126colin at 2010-07-12 19:53:18
Solution: Radix Sort
*/
#include <iostream>
using namespace std;

int n,a[4],i,j;

int main() {
	scanf("%d",&n);
	for (i=1;i<=n;i++) scanf("%d",&j),a[j]++;
	for (i=1;i<=3;i++) for (j=1;j<=a[i];j++) putchar(i+'0'),putchar(32);
	puts("");
}
```