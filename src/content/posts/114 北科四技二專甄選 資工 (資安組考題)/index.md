---
title: 114 北科四技二專甄選 資工 (資安組考題)
published: 2025-06-23
tags: [統測甄選]
category: Life
draft: false
image: 'image.png'
---



## 前言

統測成績出來之後跑去投推甄，去網路上找北科資工資安跟一般組考試的考古題都找不到，於是就誕生了這篇文章QQ，喔對ㄌ，我這一屆(114)，一般組跟資安組的考題都是程式設計的筆試題目，選擇題的部分有 20 題，然後 40 分鐘，前面 10 題 有 5 個選項(A,B,C,D,E)讓你選，後面 10 題是一堆選項對應一個答案讓你選(A~K)，ps: 這篇只有部分題目，而且只是雷同並不是完全一模一樣喔!

## 題目

### 1.

 有關指標的程式設計，如下片段程式可以在【1】的位置加入哪一行程式碼，編譯時不
會產生任何錯誤或警告訊息？
```c=
int *p;
char MyName[] = {'A'};
int MyAge;
double MyWeight;
【1】
```

( A ) p = MyName; ( B ) p =&MyName[0];
( C ) p = MyWeight; ( D ) p =&MyAge;


### 2.

在跑馬燈的設計上，可以由陣列中取出文字，並且顯示於螢幕上。如下片段程式執行完後
ShowMessage字串為 "ILoveTaiwan"，則Count的初始值應為下列哪一個？
```c=
const int Count = ? ;
char Dictionary[50] = "IWhLoaorevYTeouTW5aM7iynwyuaTYn?";
char ShowMessage[12] ={0};
char *P = &Dictionary[0];
int Index = 0, Search = 0;
for(Index =0; Index < 11 ; Index++)
{
    ShowMessage[Index] = P[Search];
    Search += Count;
}
```

( A ) 0 ( B ) 1 ( C ) 2 ( D ) 3


### 3. (分兩小題)

小芳在一個原本可以編譯(Compile)成功的程式中，在 main( )主程式內再加入行號 1 至
行號 6 的程式碼，但加入後發生編譯錯誤的情況。

```c=
#define Value1 100
#define Value2 (Value1 - 1)
const int Value3;
int CheckValue = 0;
Value3 = Value2;
CheckValue = Value1 + Value3;
```

3.1 小芳刪除行號 1 至行號 5 中的哪一個部分後，可以使程式編譯成功？
( A ) (Value1 - 1) ( B ) Value3 = Value2;
( C ) const ( D ) #define Value2 (Value1 - 1)


3.2 程式修正後，當程式執行完行號 6 的時候，CheckValue 的值為下列何者？
( A ) 200 ( B ) 199 ( C ) 198 ( D ) 100

### 4.

曉華想要知道三角函數 sin(x)在x=0之後遞增的變化情形，寫了如下的C語言程式碼，
卻發現迴圈內行號 8 和行號 9 的程式碼只執行了一次，下列哪一種修改程式的方式可以讓
迴圈內的程式碼多執行幾次？(提示：sin(1)=0.8415)
```c=
#include <stdio.h>
#include <math.h>
int x = 100;
int main(){
int x = 0;
double y = 0.0;
do{
y = 10*sin(x);
printf("x=%d, y=%lf\n", x, y);
 } while(++x <= y);
     printf("end of program\n");
     return 0;
 }
```
( A ) 把行號3中的x=100改為x=0 ( B ) 把行號10中的`++x`改為`x++`
( C ) 把行號6中y的初始值改為–1.0 ( D ) 把行號3中x的初始值改為1

### 5.

曉華寫了下列一段 C 語言程式，想要測試程式執行時如何透過作業系統的終端機
(Console )指令取得參數(Arguments)，但發現無法成功進行編譯，應採取下列哪一個方案
來解決這個問題？
```c=
1
#include <stdio.h>
//void sub(int i, char *s);
int main(int argc, char *argv[]) {
sub(argc, argv[2]);
return 0;
}

void sub(int i, char *s){
 printf("total %d arguments, and the 2nd one is %s\n", i, s);
11 }
```
( A ) 將行號4中main( int argc, char *argv[] )改為main()
( B ) 去掉行號3最前面的註解標記//
( C ) 將行號1的空白行刪除
( D ) 在行號1新增#include <stdlib.h>


### 6.

下面哪一行不會被執行到?

```c=
int f(int n) {
    if (n > 17) 
        n += 5 ;
    while (n >= 23)
        n -= 6 ;
    if (n > 17)
        n += 2 ;
    return n ;
}

```

( A ) 2 ( B ) 5 ( C ) 6 ( D ) 7


### 7.

輸出是多少

```c=
#include <stdio.h>

int main()
{
        int arr [3][3] = {{1,2,3} , {4,5,6} , {7,8,9}};
        printf("%d",arr[1][2]);

}
```

( A ) 6 ( B ) 1 ( C ) 7 ( D ) 8

### 8.

![image](https://hackmd.io/_uploads/BkyyvnLNlg.png)

這題不是原本不是長這樣，但我懶得寫測資，所以就從網路上幹圖ㄌ : D

### 9.

![image](https://hackmd.io/_uploads/S1TUv3LExl.png)

同上

### 10.

```c=
#include <stdio.h>

int main() {
    int a = 1, b = 2, c = 3, d = 4;
    int i = 0;

    do {
        a += 1;
        b += a;
        c = b - a;
        d = a * c;
        i++;
    } while (d < c); 


    return 0;
}

```

問你 i 的值是多少

## 結語

這邊大概只有全部考題的一半，我好像不是很會記這種東西，抱歉na

## Reference

https://web1.tcte.edu.tw/EXAM/111_4y/

https://web1.tcte.edu.tw/EXAM/112_4y/

https://web1.tcte.edu.tw/EXAM/113_4y/

https://web1.tcte.edu.tw/EXAM/114_4y/

https://hackmd.io/@apcser/B1N5GYEto

https://hackmd.io/@howkii-studio/apcs_exercise_programming

https://apcs.csie.ntnu.edu.tw/wp-content/uploads/2018/12/1060304APCSconcept.pdf