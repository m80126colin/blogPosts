title: '[解題報告][ZeroJudge][d635] 幸運777？luck'
date: 2010-07-13 17:37:48
tags:
- 程式碼
- C++
categories:
- 程式碼
- ZeroJudge
---

![](/blog/img/20100713-173748-1.png)

<!-- more -->

``` cpp
/*
Problem: d635
Language: C++
Result: AC (1ms, 708KB) on ZeroJudge
Author: m80126colin at 2010-07-13 17:30:31
Code Lenth: 90 Bytes
Solution: Binary conversion
*/
#include <iostream>
main() {for (int n;scanf("%d",&n),n>=0;printf("%o\n",n));puts("-1");}
```