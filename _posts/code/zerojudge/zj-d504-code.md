title: '[解題報告][ZeroJudge][d504] 第五題：超立方體的路徑問題'
date: 2009-11-12 21:27:38
tags:
- C++
- Dynamic Programming
- 位元運算
categories:
- 程式碼
- ZeroJudge
---

![](/blog/img/20091112-212738-1.jpg)

<spen style="color: red;">下面捏很大，請小心</span>

<!-- more -->

解題方法：位元運算 + DP

``` cpp
#include <iostream>
using namespace std;

int a[512],n,m,i,j;

int main() {
	while (cin>>n&&n) {
		m=1<<n;
		int result[512]={0};
		for (i=0;i<m;i++) scanf("%d",&a[i]);
		result[0]=a[0];
		for (i=0;i<m;i++) {
			for (j=0;j<n;j++) result[i^(1<<j)]=max(result[i^(1<<j)],result[i]+a[i^(1<<j)]);
		}
		cout<<result[m-1]<<endl;
	}
}
```