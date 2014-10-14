title: '[USACO][TEXT] Section 1.1 Submitting Solutions'
date: 2009-10-24 16:20:09
tags:
- USACO
categories:
- 翻譯
- USACO
---

這個名為「test」的題目是最簡單的程式題，它要求您讀入一對整數從名為「`test.in`」的文件，並且輸出在「`test.out`」的檔案裡。

以下是 C 語言的程式碼。注意：請使用「`exit(0);`」。

<!-- more -->

``` c
/*
ID: your_id_here
LANG: C
TASK: test
*/
#include <stdio.h>
main () {
    FILE *fin  = fopen ("test.in", "r");
    FILE *fout = fopen ("test.out", "w");
    int a, b;
    fscanf (fin, "%d %d", &a, &b); /* 這裡輸入兩個整數 */
    fprintf (fout, "%d\n", a+b);
    exit (0);
}
```

以下是 C++ 的程式碼。注意：請使用「return(0);」。

``` cpp
/*
ID: your_id_here
PROG: test
LANG: C++
*/
#include <iostream>
#include <fstream>
#include <string>
using namespace std;
int main() {
    ofstream fout ("test.out");
    ifstream fin ("test.in");
    int a, b;
    fin >> a >> b;
    fout << a+b << endl;
    return 0;
}
```

以下是Pascal的：

``` delphi
{
ID: your_id_here
PROG: test
LANG: PASCAL
}
Program Test;
Var fin, fout: text;
    a, b: word;
Begin
    Assign(fin, 'test.in'); Reset(fin);
    Assign(fout, 'test.out'); Rewrite(fout);
    Readln(fin, a, b);
    Writeln(fout, a+b);
    Close(fout);
End.
```

以下是 Java 的：

``` java
/*
ID: your_id_here
LANG: JAVA
TASK: test
*/
import java.io.*;
import java.util.*;
class test {
  public static void main (String [] args) throws IOException {
    // 使用BufferedReader會比RandomAccessFile快
    BufferedReader f = new BufferedReader(new FileReader("test.in"));
         //輸入要讀取的檔案名稱
    PrintWriter out = new PrintWriter(new BufferedWriter(new FileWriter("test.out")));
    //StringTokenizer比readLine/split快得多
    StringTokenizer st = new StringTokenizer(f.readLine());
 //讀取整行
    int i1 = Integer.parseInt(st.nextToken());    //第一個整數
    int i2 = Integer.parseInt(st.nextToken());    //第二個整數
    out.println(i1+i2);                           //輸出結果
    out.close();                                  //結束輸出
    System.exit(0);                               //切勿忘記這個！
  }
}
```

重要提示：BufferedReader 和 StringTokenizer 比其他輸入輸出更有效率。