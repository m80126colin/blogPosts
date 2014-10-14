title: '[解題報告][ZeroJudge][b200] E. 幼稚的災難'
date: 2009-11-12 21:29:24
tags:
- C++
- Q-matrix
categories:
- 程式碼
- ZeroJudge
---

{% owl-img img/20091112-212924-1.jpg %}

目前寫過最噁心的解法（雖然看起來滿簡潔的）
如果下次寫SCC的話，
可能又要破紀錄了

<!-- more -->

解題方法: Q-matrix，再利用 Fast.exp() 加速 {% math O(\log{(t)} \times{\text{sizeof}(\text{matrix})^3)} %}

``` cpp
#include <iostream>
using namespace std;

long long m,d;

struct matrix{
	long long a[2][2];
}mtx[32];

int main() {
	int i,j,k;
	int n,t,a;
	long long temp;
	for (cin>>a;a;a--) {
		cin>>n>>m>>d>>t;
		mtx[1].a[0][0]=0,mtx[1].a[0][1]=1;
		mtx[1].a[1][0]=2,mtx[1].a[1][1]=2;
		if (t%2) {
			temp=m;
			m=(mtx[1].a[0][0]*m+mtx[1].a[0][1]*d)%n;
			d=(mtx[1].a[1][0]*temp+mtx[1].a[1][1]*d)%n;
		}
		t/=2;
		for (i=2;t;i++,t/=2) {
			for (j=0;j<2;j++) {
				for (k=0;k<2;k++) {
					mtx[i].a[j][k]=0;
					for (int l=0;l<2;l++) mtx[i].a[j][k]+=mtx[i-1].a[j][l]*mtx[i-1].a[l][k];
					mtx[i].a[j][k]%=n;
				}
			}
			if (t%2) {
				temp=m;
				m=(mtx[i].a[0][0]*m+mtx[i].a[0][1]*d)%n;
				d=(mtx[i].a[1][0]*temp+mtx[i].a[1][1]*d)%n;
			}
		}
		cout<<m<<" "<<d<<endl;
	}
}
```