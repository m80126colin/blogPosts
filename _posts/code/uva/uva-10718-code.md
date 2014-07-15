title: '[解題報告][UVa][10718] Bit Mask'
date: 2012-05-30 10:09:00
tags:
- C++
- Greedy
categories:
- 程式碼
- UVa
---

題目大意：給你一個 N，再給下限 L，上限 U，問從 [L, U] 區間中取得最小一個數 M，使得 N or M 最大。

下有雷 ...........

<!-- more -->

解法：Greedy

儘量讓 N 中為 0 的位元，M 為 1；N 為 1 的位元， M 為 0。
因此從高位往低位檢查，
如果 N 為 1（M 儘量為 0），若 M 不能為 0，則必是因為 M 為 1 仍小於 L；
如果 N 為 0（M 儘量為 1），若 M 不能為 1，則必是因為 M 為 0 仍大於 U（此時不可能），
換句話說，M 為 1 時，M 需不大於 U。


Code：

```
#include <stdio.h>
#include <iostream>
#define BIT(i) (1LL << i)
#define OR(x,i) (x | BIT(i))
#define AND(x,i) (x & BIT(i))
using namespace std;

int main()
{
	long long n, L, U, ans, i;
	while (scanf("%lld%lld%lld", &n, &L, &U) != EOF)
	{
		ans = 0;
		for (i = 31; i >= 0; i--)
			if ( OR(ans, i) <= (AND(n, i) ? L: U) ) ans |= BIT(i);
		printf("%lld\n", ans);
	}
}
```