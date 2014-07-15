title: '[USACO][PROB] Section 1.1 Broken Necklace'
date: 2009-10-25 15:09:24
tags:
- USACO
categories:
- 翻譯
- USACO
---

現在你有一條有N個小珠子的項鍊 (3 <= N <= 350)，這些珠子有可能是紅色、藍色或者是白色。以下是兩條 N = 29 的例子：

<!-- more -->

``` text

                1 2                               1 2
            r b b r                           b r r b
          r         b                       b         b
         r           r                     b           r
        r             r                   w             r
       b               r                 w               w
      b                 b               r                 r
      b                 b               b                 b
      b                 b               r                 b
       r               r                 b               r
        b             r                   r             r
         b           r                     r           r
           r       r                         r       b
             r b r                             r r w
            Figure A                         Figure B
                        r red bead
                        b blue bead
                        w white bead
```

在圖中已經標明第一顆珠子和第二顆珠子的位置，而我們為了以字串表示項鍊，將藍色珠子表示為 b、紅色珠子為 r，以下是圖 A 的表示法：

```
brbrrrbbbrrrrrbrrbbrbbbbrrrrb
```

假設你從項鍊的某處切斷，然後從斷裂處兩邊開始收集相同顏色的珠子，直到遇到不同顏色的為止 (兩邊所收集的顏色可能不一樣)。請求出從哪邊切斷可以收集到最多的小珠子。

### 範例

舉例來說，如果從圖 A 的第 9 顆和第 10 顆之間、或者是第 24 顆和第 25 顆之間斷開，最多可以收集到 8 個珠子。但是某些項鍊會像圖 B 有白色珠子，而白色珠子可以被當作紅色或藍色其中一種顏色來收集，而白色珠子以字元 w 代表。

## 名稱：beads

## 輸入格式
<table><tr><td>第一行</td><td>一個整數 N 代表珠子數</td></tr><tr><td>第二行</td><td>一行有 N 個字元的字串</td></tr></table>
 
## 範例輸入 (檔案 beads.in)

```
29
wwwbbrwrbrbrrbrbrwrwwrbwrwrrb
```

## 輸出格式

所收集的珠子的最大值。

## 範例輸出（檔案beads.out）

```
11
```

### 輸出解說

當做是把兩條相同的鍊子串在一起 (類似頭接尾的效果)，以下標出取出的 11 個珠子。

<pre>
                複製的兩條項鍊從這斷開
                             v
wwwbbrwrbrbrrbrbrwrwwrbwrwrrb|wwwbbrwrbrbrrbrbrwrwwrbwrwrrb
                       ******|*****
                       rrrrrb bbbbb  &lt;-- 指定顏色
                       rrrrr#bbbbbb  
                       5 x r  6 x b  &lt;-- 總共 11 個
</pre>