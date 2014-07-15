title: '[解題報告][ZeroJudge][d563] 等值首尾和'
date: 2010-07-09 18:55:11
tags:
- 程式碼
- C++
categories:
- 程式碼
- ZeroJudge
---

![](/blog/img/20100709-185511-1.jpg)

<!-- more -->

``` cpp
/*
Problem: d563
Language: C++
Result: AC (18ms, 1106KB) on ZeroJudge
Author: m80126colin at 2010-07-09 18:50:53
Solution: Straight Forward
*/
#include <iostream>
using namespace std;

int main() {
	int n,i,j,t,q,a[100000],ox[100000];
	scanf("%d",&n);
	for (i=t=0;i<n;i++) {
		scanf("%d",&a[i]);
		t+=a[i];
		ox[i]=t;
	}
	j=q=0;
	for (i=t=0;i<n&&j<n;i++) {
		t+=a[n-1-i];
		while (t>ox[j]&&j<n) j++;
		if (t==ox[j]) q++;
	}
	cout<<q<<endl;
}
```