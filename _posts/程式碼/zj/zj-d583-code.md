title: '[解題報告][ZeroJudge][d583] 幼稚的企鵝'
date: 2010-07-12 17:46:08
tags:
- 程式碼
- C++
categories:
- 程式碼
- ZeroJudge
---

{% owl-img img/20100712-174608-1.jpg %}

<!-- more -->

``` cpp
/*
Problem: d583
Language: C++
Result: AC (22ms, 718KB) on ZeroJudge
Author: m80126colin at 2010-07-12 17:39:11
Solution: Output Directly
*/
#include <iostream>
main() {for (int n,i,j;~scanf("%d",&n);putchar(10)) for (i=1;i<=n;i++) scanf("%d",&j),printf("%d ",i);}
```