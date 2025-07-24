---
title: THM Wonderland
published: 2025-07-23
tags: [THM,Linux]
category: PenTest
draft: false
image: 'image.png'
---

## Recon

```bash=
┌──(kali㉿kali)-[~]
└─$ rustscan -a 10.10.21.151 -r 1-65535 --ulimit 5000   -- -sC -sV -Pn
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
I scanned my computer so many times, it thinks we're dating.

[~] The config file is expected to be at "/home/kali/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.10.21.151:22
Open 10.10.21.151:80
[~] Starting Script(s)
[>] Running script "nmap -vvv -p {{port}} -{{ipversion}} {{ip}} -sC -sV -Pn" on ip 10.10.21.151
Depending on the complexity of the script, results may take some time to appear.
[~] Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-14 04:34 EDT
NSE: Loaded 157 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 04:34
Completed NSE at 04:34, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 04:34
Completed NSE at 04:34, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 04:34
Completed NSE at 04:34, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 04:34
Completed Parallel DNS resolution of 1 host. at 04:34, 0.01s elapsed
DNS resolution of 1 IPs took 0.01s. Mode: Async [#: 1, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating SYN Stealth Scan at 04:34
Scanning 10.10.21.151 [2 ports]
Discovered open port 22/tcp on 10.10.21.151
Discovered open port 80/tcp on 10.10.21.151
Completed SYN Stealth Scan at 04:34, 0.33s elapsed (2 total ports)
Initiating Service scan at 04:34
Scanning 2 services on 10.10.21.151
Completed Service scan at 04:34, 8.73s elapsed (2 services on 1 host)
NSE: Script scanning 10.10.21.151.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 04:34
Completed NSE at 04:34, 9.59s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 04:34
Completed NSE at 04:34, 1.57s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 04:34
Completed NSE at 04:34, 0.00s elapsed
Nmap scan report for 10.10.21.151
Host is up, received user-set (0.30s latency).
Scanned at 2025-07-14 04:34:05 EDT for 20s

PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 60 OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 8e:ee:fb:96:ce:ad:70:dd:05:a9:3b:0d:b0:71:b8:63 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDe20sKMgKSMTnyRTmZhXPxn+xLggGUemXZLJDkaGAkZSMgwM3taNTc8OaEku7BvbOkqoIya4ZI8vLuNdMnESFfB22kMWfkoB0zKCSWzaiOjvdMBw559UkLCZ3bgwDY2RudNYq5YEwtqQMFgeRCC1/rO4h4Hl0YjLJufYOoIbK0EPaClcDPYjp+E1xpbn3kqKMhyWDvfZ2ltU1Et2MkhmtJ6TH2HA+eFdyMEQ5SqX6aASSXM7OoUHwJJmptyr2aNeUXiytv7uwWHkIqk3vVrZBXsyjW4ebxC3v0/Oqd73UWd5epuNbYbBNls06YZDVI8wyZ0eYGKwjtogg5+h82rnWN
|   256 7a:92:79:44:16:4f:20:43:50:a9:a8:47:e2:c2:be:84 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBHH2gIouNdIhId0iND9UFQByJZcff2CXQ5Esgx1L96L50cYaArAW3A3YP3VDg4tePrpavcPJC2IDonroSEeGj6M=
|   256 00:0b:80:44:e6:3d:4b:69:47:92:2c:55:14:7e:2a:c9 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAsWAdr9g04J7Q8aeiWYg03WjPqGVS6aNf/LF+/hMyKh
80/tcp open  http    syn-ack ttl 60 Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Follow the white rabbit.
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 04:34
Completed NSE at 04:34, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 04:34
Completed NSE at 04:34, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 04:34
Completed NSE at 04:34, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.60 seconds
           Raw packets sent: 2 (88B) | Rcvd: 2 (88B)


```


## Exploit

80 port 點進去

![image](https://hackmd.io/_uploads/SyP8WrMIxe.png)

gobuster 

```bash=
┌──(kali㉿kali)-[~]
└─$ gobuster dir -u 'http://10.10.21.151/' -w /usr/share/wordlists/dirb/big.txt -t 100
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.21.151/
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/img                  (Status: 301) [Size: 0] [--> img/]
/poem                 (Status: 301) [Size: 0] [--> poem/]
/r                    (Status: 301) [Size: 0] [--> r/]
Progress: 20469 / 20470 (100.00%)
===============================================================
Finished
===============================================================
```

重複掃描 `/r` 路徑，掃到最後變成 `/r/a/b/b/i/t` ，然後就遇到超級通靈的東西。 

![image](https://hackmd.io/_uploads/Bk6curMUeg.png)

alice:HowDothTheLittleCrocodileImproveHisShiningTail 這一坨是 ssh 的密碼



連上去先用 `sudo -l` 看可以用哪些指令，發現有個 python 檔案讓我們使用，但是沒辦法做寫入

![image](https://hackmd.io/_uploads/SyrwqHnIxe.png)

![image](https://hackmd.io/_uploads/S1gdcS3Uel.png)


之後用 linPeas 掃資料

![Screenshot 2025-07-20 at 11.46.23 AM](https://hackmd.io/_uploads/BJTcU198lg.png)

![Screenshot 2025-07-20 at 11.52.14 AM](https://hackmd.io/_uploads/HkyZu1cIxl.png)

直接用 capacity 砸下去發現權限不夠

![Screenshot 2025-07-20 at 12.14.37 PM](https://hackmd.io/_uploads/ryp4pJ9Iee.png)

懷疑要去登入其他使用者才可以使用 `perl`

![Screenshot 2025-07-20 at 12.15.14 PM](https://hackmd.io/_uploads/ryeD6yc8el.png)

發現有這些使用者，嘗試用 hydra 搭配 rockyou.txt 去爆破，最後爆破失敗

回頭看看 `sudo -l` 所顯示的可執行 python 檔，先做假設，在一般情況下如果把檔案名稱叫做 `module name` 的話，並且有使用到套件上的東西的話，檔案會不讓你執行，  

![image](https://hackmd.io/_uploads/ryXPiShLgg.png)

參考 https://docs.python.org/zh-tw/3.13/tutorial/modules.html 會發現他會先在你同一個資料夾上尋找 `module` 的細節，接著再去 `pythonmath`，如果沒有的話才會去 `site-packages`

![image](https://hackmd.io/_uploads/rywynS2Uxg.png)

知道這個特性之後，我們可以寫一個 reverse shell 進去，然後 call 題目說我們可以執行的腳本，那這邊踩了一個雷是他沒有跟我想像中一樣變成 rabbit 這個 user，結果是我本來就用到 alice 這個 user 去執行，而不是 rabbit ...

![image](https://hackmd.io/_uploads/ryY80H2Ixe.png)

![image](https://hackmd.io/_uploads/Sk4vCH2Uxx.png)


成功轉換使用者 

![image](https://hackmd.io/_uploads/B1io0H3Ugx.png)

進去 rabbit user 之後有個 `teaParty` 的檔案

![image](https://hackmd.io/_uploads/Hyo-gUn8xe.png)

發現他有 suid ，但是執行的時候會跳出這些

![image](https://hackmd.io/_uploads/rJTXg83Ill.png)

透過 scp 傳回來，並且用 ida 分析會發現這些

![image](https://hackmd.io/_uploads/S1znGL2Ill.png)

看完之後發現我們可以透過改 date 環境變數的方式去 get 另一個 user 的權限

先開一個 date 檔案

```bash=
echo "/bin/sh" > date
chmod 777 ./date
```

把他丟到環境變數

```bash=
export PATH=.:$PATH
```

執行他

```bash=
./teaParty
```

get hatter user

![image](https://hackmd.io/_uploads/By3-Q8h8ge.png)

拿到 password.txt

![image](https://hackmd.io/_uploads/HyjU7U38lx.png)

然後用 ssh 連上去

![image](https://hackmd.io/_uploads/Syri7L2Uee.png)

然後猜測說應該可以用前面找到的 perl 去 get root

![image](https://hackmd.io/_uploads/HkrDE8n8ge.png)

實測之後發現成功!

> Get root Flag: thm{Twinkle, twinkle, little bat! How I wonder what you’re at!}

然後在 `/root` 找到 `user.txt`

> Get user Flag: thm{"Curiouser and curiouser!"}

## Pwned!

![image](https://hackmd.io/_uploads/HJpC4Lh8lx.png)
