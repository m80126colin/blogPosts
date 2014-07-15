title: '[解題報告][ZeroJudge][b016] Mitlab'
date: 2011-10-27 02:24:00
tags:
- C++
- 矩陣運算
categories:
- 程式碼
- ZeroJudge
---

以下有雷

<!-- more -->

# 矩陣乘法

這題主要是在模擬矩陣運算
加減法不用說

矩陣乘法定義：

```
A.size() = m * p
B.size() = p * n

procedure Matrix_Multiplication ( Matrix A, Matrix B )

	create C as m * n Matrix

	for i = 1 to m
		for j = 1 to n
			for k = 1 to p
				c[i, j] = c[i, j] + a[i, k] * b[k, j]
			end for
		end for
	end for

	return C

end procedure
```

# 反矩陣

## 轉置矩陣

反矩陣更好玩，
先定義轉置矩陣 {% math A^T %}：

```
A.size() = m * n

procedure Transpose ( Matrix A )

	create At as n * m Matrix

	for i = 1 to m
		for j = 1 to n
			At[i, j] = A[j, i]
		end for
	end for

	return At

end procedure
```

## 行列式

再定義行列式 det A：

```
A.size() = n * n

procedure Determinant ( Matrix A )

	create Det = 0 as integer
	create Tmp as integer

	for i = 1 to n

		Tmp = 1
		for k = 1 to n
			Tmp = Tmp * a[(i + k - 1) mod n + 1, j] 
		end for
		Det = Det + Tmp

		Tmp = 1
		for k = 1 to n
			Tmp = Tmp * a[(i - k + n - 1) mod n + 1, j]
		end for
		Det = Det - Tmp

	end for

	return Det

end procedure
```

## 餘子陣 (Minor)

假設原 n * n 方陣 A ， 餘子陣(i, j) 代表原方陣去掉第 i 行第 j 列所得 (n - 1) * (n - 1) 的子方陣記為 M(i, j)

Pseudo Code:

```
A.size() = n * n

procedure Minor ( integer x, integer y )

	create M as (n-1) * (n-1) Matrix

	for i = 1 to x - 1
		for j = 1 to y - 1
			M[i, j]  = A[i, j]
		end for

		for j = y to n - 1
			M[i, j] = A[i, j + 1]
		end for
	end for

	for i = x to n - 1
		for j = 1 to y - 1
			M[i, j]  = A[i + 1, j]
		end for

		for j = y to n - 1
			M[i, j] = A[i + 1, j + 1]
		end for
	end for

	return M

end procedure
```

## 餘因子矩陣 (Cofactor)

接著我們要求餘因子矩陣 C(i, j)

定義為：

```
C(i, j) = ( -1 ) ^ ( i + j ) * M(i, j)
```

將 A 所有餘因子矩陣的行列式值求出來，意即：

```
create B as n * n Matrix

for i = 1 to n
	for j = 1 to n
		b[i, j] = Determinant( C[i, j] )
	end for
end for
```

## 反矩陣

此時矩陣 B 的轉置矩陣 (又稱古典伴隨矩陣) 為 A 的反矩陣 乘上 det A

```
A ^ (-1) = Transpose( B ) / Determinant( A )
```

# 程式碼

神秘解法wwwwwwww
或是反矩陣可用高斯消去法推求
剩下格式部分只要使用 Parser 就搞定

``` cpp
#include <stdio.h>
#include <ctype.h>
#include <string.h>
#include <iostream>
#define MAX 30
using namespace std;

struct Matrix
{
	int m, n;
	int ele[MAX][MAX];
	Matrix () {}
	Matrix (int m, int n, int d): m(m), n(n) {
		int i, j;
		for (i = 0; i < m; i++) {
			for (j = 0; j < n; j++) {
				ele[i][j] = d;
			}
		}
	}
	inline Matrix operator + (const Matrix &b) {
		Matrix a = (*this);
		int i, j;
		for (i = 0; i < m; i++) {
			for (j = 0; j < n; j++) {
				a.ele[i][j] += b.ele[i][j];
			}
		}
		return a;
	}
	inline Matrix operator - (const Matrix &b) {
		Matrix a = (*this);
		int i, j;
		for (i = 0; i < m; i++) {
			for (j = 0; j < n; j++) {
				a.ele[i][j] -= b.ele[i][j];
			}
		}
		return a;
	}
	inline Matrix operator * (const Matrix &b) {
		Matrix a = Matrix(m, b.n, 0);
		int i, j, k;
		for (i = 0; i < m; i++) {
			for (j = 0; j < b.n; j++) {
				for (k = 0; k < n; k++) {
					a.ele[i][j] += ele[i][k] * b.ele[k][j];
				}
			}
		}
		return a;
	}
	inline Matrix trans() {
		Matrix a = Matrix(n, m, 0);
		int i, j;
		for (i = 0; i < m; i++) {
			for (j = 0; j < n; j++) {
				a.ele[i][j] = ele[j][i];
			}
		}
		return a;
	}
	inline int det() {
		if (n == 1) return ele[0][0];
		if (n == 2) return ele[0][0] * ele[1][1] - ele[1][0] * ele[0][1];
		int i, j, tmp, d = 0;
		for (i = 0; i < m; i++) {
			tmp = 1;
			for (j = 0; j < n; j++) {
				tmp *= ele[j][(i+j) % n];
			}
			d += tmp;
			tmp = -1;
			for (j = 0; j < n; j++) {
				tmp *= ele[j][(i-j+n) % n];
			}
			d += tmp;
		}
		return d;
	}
	inline Matrix minor(int x, int y) {
		Matrix a = Matrix(m - 1, n - 1, 0);
		int i, j;
		for (i = 0; i < m; i++) {
			for (j = 0; j < n; j++) {
				a.ele[i - (i > x)][j - (j > y)] = ele[i][j];
			}
		}
		return a;
	}
	inline Matrix inverse() {
		Matrix a = Matrix(n, m, 0);
		int i, j;
		for (i = 0; i < m; i++) {
			for (j = 0; j < n; j++) {
				a.ele[i][j] = ( ((i+j) & 1)? -1: 1) * ((*this).minor(i, j)).det();
			}
		}
		return a.trans();
	}
	inline void output() {
		int i, j;
		putchar('[');
		for (i = 0; i < m; i++) {
			for (j = 0; j < n; j++)
				printf(" %d", ele[i][j]);
			if(i < m - 1)
				printf(" ;");
		}
		puts(" ]");
		return;
	}
}m[26];

char s[3000];

inline void out() {
	int i;
	for (i = 0; i < 26; i++)
		m[i].output();
	return;
}

inline void sol()
{
	int x = s[0] - 'a', i, j, k;
	if(s[4] == '[') {
		i = j = 0;
		int d = 0, p = 1;
		for (k = 6; s[k]; k++) {
			if (isdigit(s[k]) || s[k] == '-') {
				if(s[k] == '-')
					p = -1, k++;
				else
					p = 1;
				while (isdigit(s[k]))
					d = d * 10 + (s[k] - '0') * p, k++;
				m[x].ele[i][j++] = d;
				d = 0;
			}
			else if (s[k] == ';')
				j = 0, i++;
		}
		m[x].m = i + 1;
		m[x].n = j;
	}
	else {
		if (strlen(s) == 5) {
			m[x] = m[s[4] - 'a'];
			return;
		}
		Matrix a = m[s[4] - 'a'], b = m[s[8] - 'a'];
		switch (s[6]) {
			case '+':
				m[x] = a + b;
				break;
			case '-':
				m[x] = a - b;
				break;
			case '*':
				m[x] = a * b;
				break;
			case '/':
				m[x] = a * b.inverse();
				break;
			case '\\':
				m[x] = a.inverse() * b;
				break;
			default:
				break;
		}
	}
	return;
}

int main() {
	int i, c = 0;
	while (gets(s)) {
		if (c++) puts("");
		for (i = 0; i < 26; i++) m[i].n = m[i].m = 0;
		if(!s[0]) {
			out();
			continue;
		}
		sol();
		while (gets(s)) {
			if(!s[0]) break;
			else sol();
		}
		out();
	};
}
```