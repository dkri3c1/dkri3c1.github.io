---
title: PicoCTF 2025  Writes up
published: 2025-05-01
tags: [CTF]
category: Cyber Security
draft: false
image: 'image.png'
---


## Before

這一篇是統測前打ㄉ，現在每天都過得好開心，終於可以做自己想做的事ㄌ，這場是跟 Remote Computer Explosion 一起打的 ，而我主要打ㄉ領域是 Forensics，大概解了4.5題，.5 的部分是因為那一題是和 [p23](https://github.com/pictures2333)一起解ㄉ至於 Misc 我開賽隔一天上去看之後就被打光光ㄌ，都沒有摸到 QQ

這邊附上一個 Score Board

![image](https://hackmd.io/_uploads/ByEd8U8hJx.png)


## Forensics

### Ph4nt0m 1ntrud3r

![image](https://hackmd.io/_uploads/BJJ1dQTj1x.png)

- hint1 : Filter your packets to narrow down your search.
- hint2 : Attacks were done in timely manner.
- hint3 : Time is essential

題目給了一個封包打開來之後會發現 content 的部分有一些 base64 的編碼

![image](https://hackmd.io/_uploads/BkJEOQpi1g.png)

然後題目的提示說跟時間有關，所以就用 Time 讓他排序好

![image](https://hackmd.io/_uploads/SJgndQ6o1l.png)

大概從 `Time 0.002212` 開始會出現 Flag 經過編碼之後的東東

![image](https://hackmd.io/_uploads/SJTZKXpsyg.png)

> flag: picoCTF{1t_w4snt_th4t_34sy_tbh_4r_e5e8c78d}

### RED

![image](https://hackmd.io/_uploads/BkPuKmpoyg.png)

- hint1 : The picture seems pure, but is it though?
- hint2 : Red?Ged?Bed?Aed?
- hint3 : Check whatever Facebook is called now.

這題一看就是 Steganography ，於是就把他丟去萬用工具 [Aperi'Solve](https://www.aperisolve.com/) 上

接著就看到一串經過 base64 編碼的東西，把它拿去解碼就拿到 Flag 了

![image](https://hackmd.io/_uploads/B1tYGO6oyx.png)

![image](https://hackmd.io/_uploads/rkCofdas1g.png)

> picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}

### flags are stepic

![image](https://hackmd.io/_uploads/Sy3zsII2Je.png)

- hint1: in the country that doesn't exist, the flag persists

這一題是隊友 [hokak](https://asia-hokak.github.io/) 解出來ㄉ，好像是 LSB 題，隊友根本通靈大師 >_<

至於圖片的部分是這張

![image](https://hackmd.io/_uploads/HkmDjUI21g.png)

這邊附上他的解題腳本

```py=
from PIL import Image
from Crypto.Util.number import *

img = Image.open('upz.png')


cutted = img.crop((0, 0, 90, 1))


pixels = cutted.load()

flag = ''
for i in range(90):
    pixel = pixels[i, 0]
    flag += "".join(str(pixel[x] & 1) for x in range(3))



for i in range(0, len(flag), 9):
    # print(flag[i:i+8])
    print(chr(int(flag[i:i+8], 2)), end="")
```



### Bitlocker-1

![image](https://hackmd.io/_uploads/SJCxQuasJg.png)


- hint: Hash cracking

下載之後會有一個 .dd 的檔案，題目說要拿去 hash crack，所以我就跑去 Github 上找到了這個 [工具](https://github.com/e-ago/bitcracker) 可以把 bitlocker 的值 dump 下來

```bash=
┌──(kali㉿kali)-[~/picoctf/bitcracker/build]
└─$ ./bitcracker_hash -o a -i ../../bitlocker-1.dd

---------> BitCracker Hash Extractor <---------
Encrypted device ../../bitlocker-1.dd opened, size  100.00 MB

************ Signature #1 found at 0x3 ************
Version: 8
Invalid version, looking for a signature with valid version...

************ Signature #2 found at 0x2195000 ************
Version: 2 (Windows 7 or later)

=====> VMK entry found at 0x21950c5
Encrypted with Recovery Password (0x21950e6)
Searching for AES-CCM (0x2195102)...
        Offset 0x2195195.... found! :)
======== RP VMK #0 ========
RP Salt: 2b71884a0ef66f0b9de049a82a39d15b
RP Nonce: 00be8a46ead6da0106000000
RP MAC: a28f1a60db3e3fe4049a821c3aea5e4b
RP VMK: a1957baea68cd29488c0f3f6efcd4689e43f8ba3120a33048b2ef2c9702e298e4c260743126ec8bd29bc6d58

=====> VMK entry found at 0x2195241
Encrypted with User Password (0x2195262)
VMK encrypted with AES-CCM
======== UP VMK ========
UP Salt: cb4809fe9628471a411f8380e0f668db
UP Nonce: d04d9c58eed6da010a000000
UP MAC: 68156e51e53f0a01c076a32ba2b2999a
UP VMK: fffce8530fbe5d84b4c19ac71f6c79375b87d40c2d871ed2b7b5559d71ba31b6779c6f41412fd6869442d66d

************ Signature #3 found at 0x2c1d000 ************
Version: 2 (Windows 7 or later)

=====> VMK entry found at 0x2c1d0c5
Encrypted with Recovery Password (0x2c1d0e6)
Searching for AES-CCM (0x2c1d102)...
        Offset 0x2c1d195.... found! :)

This VMK has been already stored...quitting to avoid infinite loop!

User Password hash:
$bitlocker$0$16$cb4809fe9628471a411f8380e0f668db$1048576$12$d04d9c58eed6da010a000000$60$68156e51e53f0a01c076a32ba2b2999afffce8530fbe5d84b4c19ac71f6c79375b87d40c2d871ed2b7b5559d71ba31b6779c6f41412fd6869442d66d

Recovery Key hash #0:
$bitlocker$2$16$2b71884a0ef66f0b9de049a82a39d15b$1048576$12$00be8a46ead6da0106000000$60$a28f1a60db3e3fe4049a821c3aea5e4ba1957baea68cd29488c0f3f6efcd4689e43f8ba3120a33048b2ef2c9702e298e4c260743126ec8bd29bc6d58

Output file for user password attack: "a/hash_user_pass.txt"

Output file for recovery password attack: "a/hash_recv_pass.txt"

```

dump 出來之後可以用 hashcat 去解他，那這邊因為我解過不給我解ㄌ，所以我就直接丟結果

```bash=
┌──(kali㉿kali)-[~/picoctf/bitcracker/build/a]
└─$ hashcat -m 22100 hash_user_pass.txt /usr/share/wordlists/rockyou.txt  --show
$bitlocker$0$16$cb4809fe9628471a411f8380e0f668db$1048576$12$d04d9c58eed6da010a000000$60$68156e51e53f0a01c076a32ba2b2999afffce8530fbe5d84b4c19ac71f6c79375b87d40c2d871ed2b7b5559d71ba31b6779c6f41412fd6869442d66d:jacqueline
```
搞出密碼之後我們要做的就是想辦法把那一個硬碟掛上去 Windows，這邊我用了 [osfmount](https://www.osforensics.com/tools/mount-disk-images.html) 當作掛載的工具

![image](https://hackmd.io/_uploads/SkWRu8Royx.png)

選完檔案之後就一直按 next 

![image](https://hackmd.io/_uploads/S12ytURskg.png)

到這個步驟之後就選 physical disk Emulation 然後 mount

![image](https://hackmd.io/_uploads/rkXmFURsye.png)

掛載成功ㄌ，最後就輸入剛剛的密碼 `jacqueline` ，就會解開檔案然後拿到 flag ㄌ uwu

![image](https://hackmd.io/_uploads/Bk5uF8Rokl.png)

> Flag: picoCTF{us3_b3tt3r_p4ssw0rd5_p15!_3242adb1}

### Event-Viewing

![image](https://hackmd.io/_uploads/ryDTYLAiJx.png)

- hint1: Try to filter the logs with the right event ID
- hint2: What could the software have done when it was ran that causes the shutdowns every time the system starts up?

敘述說到受害者下載了一個惡意檔案，於是我們就去看 `Installer` 的 `EventID`，會發現說 `EventID=1033` 的部分會是 `Installer`，然後就找到其中一段 flag

![image](https://hackmd.io/_uploads/r1FPYLU3ke.png)

知道 `Totally_Legit_Software` 是 `Malware` 之後，就直接用 ctrl+f 去找其他有關的紀錄，最後在 `EventID=4657` 的地方找到一串第二段 flag

![image](https://hackmd.io/_uploads/Hy11qL8nJe.png)


接下來找一下題目說的關機，通常會在 `EventID = 1074` 的地方

![image](https://hackmd.io/_uploads/ryVI_8I2kg.png)

會發現註解上有一個的東西，拿去解會拿到最後一段 flag

> Flag: picoCTF{Ev3nt_vi3wv3r_1s_a_pr3tty_us3ful_t00l_81ba3fe9}

### Bitlocker 2

這一題是我跟 p23 一起解出來ㄉ，雷點在我用的 `volatility plugin` 雖然有噴出 `fvek` 的東西，但是並不是題目要ㄉ= =

---

首先準備volatility2(透過下載原始碼，用裡面的``setup.py``安裝)，並且安裝以下的插件
https://github.com/breppo/Volatility-BitLocker/tree/master

分析``memdump.mem``，先找到要用哪個Profile
```
┌──(kali㉿kali)-[~/picoctf/bitlocker2]
└─$ sudo vol.py -f memdump.mem kdbgscan
Volatility Foundation Volatility Framework 2.6.1
**************************************************
Instantiating KDBG using: Unnamed AS Win10x64_19041 (6.4.19041 64bit)
Offset (V)                    : 0xf80251217b20
Offset (P)                    : 0x2c00b20
KdCopyDataBlock (V)           : 0xf80250b28898
Block encoded                 : Yes
Wait never                    : 0x9254cf55c00f1d99
Wait always                   : 0x3c76799783d65800
KDBG owner tag check          : True
Profile suggestion (KDBGHeader): Win10x64_19041
Service Pack (CmNtCSDVersion) : 0
Build string (NtBuildLab)     : 19041.1.amd64fre.vb_release.1912
PsActiveProcessHead           : 0xfffff80251235130 (95 processes)
PsLoadedModuleList            : 0xfffff80251241820 (186 modules)
KernelBase                    : 0xfffff80250617000 (Matches MZ: True)
Major (OptionalHeader)        : 10
Minor (OptionalHeader)        : 0
KPCR                          : 0xfffff8024f52f000 (CPU 0)
KPCR                          : 0xffffc301a8c67000 (CPU 1)
```
確定Profile使用``Win10x64_19041``

然後使用插件的``bitlocker``指令，找到memory dump中的FVEK
```
┌──(kali㉿kali)-[~/picoctf/bitlocker2]
└─$ sudo vol.py -f memdump.mem --profile=Win10x64_19041 bitlocker
Volatility Foundation Volatility Framework 2.6.1

[FVEK] Address : 0x9e8879926a50
[FVEK] Cipher  : AES 128-bit (Win 8+)
[FVEK] FVEK: 5b6ff64e4a0ee8f89050b7ba532f6256

[FVEK] Address : 0x9e887496fb30
[FVEK] Cipher  : AES 256-bit (Win 8+)
[FVEK] FVEK: 60be5ce2a190dfb760bea1ece40e4223c8982aecfd03221a5a43d8fdd302eaee

[FVEK] Address : 0x9e8874cb5c70
[FVEK] Cipher  : AES 128-bit (Win 8+)
[FVEK] FVEK: 1ed2a4b8dd0290f646ded074fbcff8bd

[FVEK] Address : 0x9e88779f1a10
[FVEK] Cipher  : AES 128-bit (Win 8+)
[FVEK] FVEK: bccaf1d4ea09e91f976bf94569761654
```

使用``--dislocker``選項把這些FVEK都dump出來
```
┌──(kali㉿kali)-[~/picoctf/bitlocker2]
└─$ mkdir fvek

┌──(kali㉿kali)-[~/picoctf/bitlocker2]
└─$ sudo vol.py -f memdump.mem --profile=Win10x64_19041 bitlocker --dislocker ./fvek
Volatility Foundation Volatility Framework 2.6.1

[FVEK] Address : 0x9e8879926a50
[FVEK] Cipher  : AES 128-bit (Win 8+)
[FVEK] FVEK: 5b6ff64e4a0ee8f89050b7ba532f6256
[DISL] FVEK for Dislocker dumped to file: ./fvek/0x9e8879926a50-Dislocker.fvek



[FVEK] Address : 0x9e887496fb30
[FVEK] Cipher  : AES 256-bit (Win 8+)
[FVEK] FVEK: 60be5ce2a190dfb760bea1ece40e4223c8982aecfd03221a5a43d8fdd302eaee
[DISL] FVEK for Dislocker dumped to file: ./fvek/0x9e887496fb30-Dislocker.fvek



[FVEK] Address : 0x9e8874cb5c70
[FVEK] Cipher  : AES 128-bit (Win 8+)
[FVEK] FVEK: 1ed2a4b8dd0290f646ded074fbcff8bd
[DISL] FVEK for Dislocker dumped to file: ./fvek/0x9e8874cb5c70-Dislocker.fvek



[FVEK] Address : 0x9e88779f1a10
[FVEK] Cipher  : AES 128-bit (Win 8+)
[FVEK] FVEK: bccaf1d4ea09e91f976bf94569761654
[DISL] FVEK for Dislocker dumped to file: ./fvek/0x9e88779f1a10-Dislocker.fvek



┌──(kali㉿kali)-[~/picoctf/bitlocker2]
└─$ ls
bitlocker-2.dd  fvek  memdump.mem  volatility

┌──(kali㉿kali)-[~/picoctf/bitlocker2]
└─$ ls fvek
0x9e887496fb30-Dislocker.fvek  0x9e8874cb5c70-Dislocker.fvek  0x9e88779f1a10-Dislocker.fvek  0x9e8879926a50-Dislocker.fvek
```

安裝dislocker
```
sudo apt update && sudo apt install dislocker -y
```

使用dislocker，用這些FVEK解密``bitlocker-2.dd``
不是每一個FVEK下去解密都可以變正常的，我這邊試到``5b6ff64e4a0ee8f89050b7ba532f6256``(``./fvek/0x9e8879926a50-Dislocker.fvek``)才是可以正常解密的那一個FVEK
```
sudo mkdir /mnt/test
sudo dislocker -V bitlocker-2.dd --fvek keys/0x9e8879926a50-Dislocker.fvek -- /mnt/test
sudo cp /mnt/test/dislocker-file ./6a50.dd
```

找一台Windows電腦，安裝OSFMount，用來掛載解密出來的映像檔
開啟OSFMount，點選``Mount new...``
![image](https://hackmd.io/_uploads/HJgnpbaAJl.png)

在這裡選擇我們剛剛解密出來的映像檔，然後按下面的Next
![image](https://hackmd.io/_uploads/SyHC6bpCkx.png)

按Next
![image](https://hackmd.io/_uploads/ryDlA-TC1g.png)

這裡要確定``Drive emulation``一定要是``Logical Drive Emulation``，然後就可以按Mount
![image](https://hackmd.io/_uploads/S1f-AWTRJx.png)

掛載成功
![image](https://hackmd.io/_uploads/H1Wr0-TCkl.png)

> Flag: picoCTF{B1tl0ck3r_dr1v3_d3crypt3d_9029ae5b}