title: '[解題報告][ZeroJudge][d575] 末日審判'
date: 2010-07-12 16:53:51
tags:
- 程式碼
- C++
categories:
- 程式碼
- ZeroJudge
---

{% owl-img img/20100712-165351-1.jpg %}

<!-- more -->

# 最佳化版本

最快秒數曾為 29ms。

``` cpp
/*
Problem: d575(fast)
Language: C++
Result: AC (35ms, 645KB) on ZeroJudge
Author: m80126colin at 2010-07-12 16:42:24
Solution: Math
*/
#import <iostream>
#define A long long
A a,b,c,d,e,f,r;
inline bool z(A& g) {  //input optimization
	while ((f=getchar())==32||f==10);
	for (g=(e=f-'-')?f-48:0;++f&&(f=getchar())-32&&f-10;g=g*10+(e?f-48:48-f));
	return f;
}
inline A y(A x) {return (x>0)?x:-x;}  //abs
main() {for (;z(a)&z(b)&z(c)&z(d)&z(r);puts((y(c-a)+y(d-b)>r?"alive":"die")));}
```

# 短碼版

``` cpp
/*
Problem: d575
Language: C++
Result: AC (90ms, 690KB) on ZeroJudge
Author: m80126colin at 2010-07-12 14:33:10
Solution: Math
*/
#include <iostream>
inline long long ab(long long x) {return x>0 ? x : -x;}
int main() {for (long long a,b,c,d,r;~scanf("%lld%lld%lld%lld%lld",&a,&b,&c,&d,&r);puts((ab(c-a)+ab(d-b) >r ? "alive" : "die"))) ;}
```