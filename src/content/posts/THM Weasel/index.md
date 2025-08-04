---
title: THM Zeno
published: 2025-08-04
tags: [THM,Windows]
category: PenTest
draft: false
image: 'image.png'
---

## Reocn

```bash=
┌──(kali㉿kali)-[~]
└─$ nmap -sC -sV -Pn 10.10.168.125
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-10 08:53 EDT
Nmap scan report for 10.10.168.125
Host is up (0.40s latency).
Not shown: 976 filtered tcp ports (no-response), 23 filtered tcp ports (host-prohibited)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 09:23:62:a2:18:62:83:69:04:40:62:32:97:ff:3c:cd (RSA)
|   256 33:66:35:36:b0:68:06:32:c1:8a:f6:01:bc:43:38:ce (ECDSA)
|_  256 14:98:e3:84:70:55:e6:60:0c:c2:09:77:f8:b7:a6:1c (ED25519)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 30.93 seconds
```

nmap 掃全部 port 太慢，又用 rustscan 掃一次

```bash=
┌──(kali㉿kali)-[~]
└─$ rustscan -a 10.10.168.125 -r 1-65535 --ulimit 5000 -- -sC -sV -Pn  
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Scanning ports faster than you can say 'SYN ACK'

[~] The config file is expected to be at "/home/kali/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.10.168.125:22
Open 10.10.168.125:12340
[~] Starting Script(s)
[>] Running script "nmap -vvv -p {{port}} -{{ipversion}} {{ip}} -sC -sV -Pn" on ip 10.10.168.125
Depending on the complexity of the script, results may take some time to appear.
[~] Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-10 08:54 EDT
NSE: Loaded 157 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 08:54
Completed NSE at 08:54, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 08:54
Completed NSE at 08:54, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 08:54
Completed NSE at 08:54, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 08:54
Completed Parallel DNS resolution of 1 host. at 08:54, 0.01s elapsed
DNS resolution of 1 IPs took 0.01s. Mode: Async [#: 2, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating SYN Stealth Scan at 08:54
Scanning 10.10.168.125 [2 ports]
Discovered open port 12340/tcp on 10.10.168.125
Discovered open port 22/tcp on 10.10.168.125
Completed SYN Stealth Scan at 08:54, 0.42s elapsed (2 total ports)
Initiating Service scan at 08:54
Scanning 2 services on 10.10.168.125
Completed Service scan at 08:54, 12.32s elapsed (2 services on 1 host)
NSE: Script scanning 10.10.168.125.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 08:54
Completed NSE at 08:54, 11.87s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 08:54
Completed NSE at 08:54, 1.56s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 08:54
Completed NSE at 08:54, 0.00s elapsed
Nmap scan report for 10.10.168.125
Host is up, received user-set (0.38s latency).
Scanned at 2025-07-10 08:54:17 EDT for 26s

PORT      STATE SERVICE REASON         VERSION
22/tcp    open  ssh     syn-ack ttl 60 OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 09:23:62:a2:18:62:83:69:04:40:62:32:97:ff:3c:cd (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDakZyfnq0JzwuM1SD3YZ4zyizbtc9AOvhk2qCaTwJHEKyyqIjBaElNv4LpSdtV7y/C6vwUfPS34IO/mAmNtAFquBDjIuoKdw9TjjPrVBVjzFxD/9tDSe+cu6ELPHMyWOQFAYtg1CV1TQlm3p6WIID2IfYBffpfSz54wRhkTJd/+9wgYdOwfe+VRuzV8EgKq4D2cbUTjYjl0dv2f2Th8WtiRksEeaqI1fvPvk6RwyiLdV5mSD/h8HCTZgYVvrjPShW9XPE/wws82/wmVFtOPfY7WAMhtx5kiPB11H+tZSAV/xpEjXQQ9V3Pi6o4vZdUvYSbNuiN4HI4gAWnp/uqPsoR
|   256 33:66:35:36:b0:68:06:32:c1:8a:f6:01:bc:43:38:ce (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBEMyTtxVAKcLy5u87ws+h8WY+GHWg8IZI4c11KX7bOSt85IgCxox7YzOCZbUA56QOlryozIFyhzcwOeCKWtzEsA=
|   256 14:98:e3:84:70:55:e6:60:0c:c2:09:77:f8:b7:a6:1c (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOKY0jLSRkYg0+fTDrwGOaGW442T5k1qBt7l8iAkcuCk
12340/tcp open  http    syn-ack ttl 60 Apache httpd 2.4.6 ((CentOS) PHP/5.4.16)
| http-methods: 
|   Supported Methods: GET HEAD POST OPTIONS TRACE
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.4.16
|_http-title: We&#39;ve got some trouble | 404 - Resource not found

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 08:54
Completed NSE at 08:54, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 08:54
Completed NSE at 08:54, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 08:54
Completed NSE at 08:54, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 26.54 seconds
           Raw packets sent: 2 (88B) | Rcvd: 2 (88B)

```

## Exploit

連上去長這樣

![Screenshot 2025-07-10 at 8.56.01 PM](https://hackmd.io/_uploads/ByCP_E6Hxx.png)

gobsuter

```bash=
┌──(kali㉿kali)-[~]
└─$ gobuster dir -u 'http://10.10.168.125:12340' -w /usr/share/wordlists/dirb/big.txt -t 100
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.168.125:12340
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.htaccess            (Status: 403) [Size: 211]
/.htpasswd            (Status: 403) [Size: 211]
/rms                  (Status: 301) [Size: 239] [--> http://10.10.168.125:12340/rms/]
Progress: 20469 / 20470 (100.00%)
===============================================================
Finished
===============================================================
```

連上去 `/rms`，看起來沒東西，所以繼續用 gobuster 掃看看

![Screenshot 2025-07-10 at 8.58.59 PM](https://hackmd.io/_uploads/Bk-QtEpHee.png)

```bash=
┌──(kali㉿kali)-[~]
└─$ gobuster dir -u 'http://10.10.168.125:12340/rms' -w /usr/share/wordlists/dirb/big.txt -t 100
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.168.125:12340/rms
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.htaccess            (Status: 403) [Size: 215]
/.htpasswd            (Status: 403) [Size: 215]
/admin                (Status: 301) [Size: 245] [--> http://10.10.168.125:12340/rms/admin/]
/connection           (Status: 301) [Size: 250] [--> http://10.10.168.125:12340/rms/connection/]
/css                  (Status: 301) [Size: 243] [--> http://10.10.168.125:12340/rms/css/]
/fonts                (Status: 301) [Size: 245] [--> http://10.10.168.125:12340/rms/fonts/]
/images               (Status: 301) [Size: 246] [--> http://10.10.168.125:12340/rms/images/]
/stylesheets          (Status: 301) [Size: 251] [--> http://10.10.168.125:12340/rms/stylesheets/]
/swf                  (Status: 301) [Size: 243] [--> http://10.10.168.125:12340/rms/swf/]
/validation           (Status: 301) [Size: 250] [--> http://10.10.168.125:12340/rms/validation/]
Progress: 20469 / 20470 (100.00%)
===============================================================
Finished
===============================================================
```

點進去先註冊，註冊完之後點到 `inbox[1]` 可以看到 `administrator` 點了餐，然後用 `administrator` 去登入下面的 `administrator` 登入頁面



![Screenshot 2025-07-10 at 9.11.56 PM](https://hackmd.io/_uploads/r1oXhN6rgx.png)

![Screenshot 2025-07-10 at 9.14.24 PM](https://hackmd.io/_uploads/rJp334prxe.png)

這邊用 hydra 試試看，發現根本不能，然後去看 wp 跟我說這個東西居然他媽是一個 Framework ，我以為他是手刻的... 真他媽牛逼

![Screenshot 2025-07-11 at 3.38.48 PM](https://hackmd.io/_uploads/HkSc1BArex.png)

發現有 SQLI 的問題，所以用 sqlmap 去爆，找到 db

```bash=
sqlmap -u 'http://10.10.177.108:12340/rms/delete-order.php?id=122' --dbs --random-agent --batch  --cookie "PHPSSID=7l2m0qq9qk26muvopausu5b1c6" 
```

![Screenshot 2025-07-11 at 4.47.10 PM](https://hackmd.io/_uploads/ry39kIArlg.png)

這題要爆好久，等了一小時 table 還沒完= = ，轉換跑道去用另一個可以 RCE 的 CVE，把裡面的 `proxxy` 刪掉


![image](https://hackmd.io/_uploads/ByEfmS-Ilg.png)

![image](https://hackmd.io/_uploads/B1xNQHW8xx.png)


Get shell 

![image](https://hackmd.io/_uploads/B1JSmBbLgl.png)

把它串成 reversed shell，那這邊因為 `bash -i ..` 用下去就會 ping 不到靶機

![image](https://hackmd.io/_uploads/HkhF8HbUel.png)

所以去 https://www.revshells.com/ 看到有沒有其他可以用的 reversed shell (因為我們的一句話木馬是 `shell_exec`，可能導致他的一些特殊符號被吃掉

找到這個

![image](https://hackmd.io/_uploads/BJ9FZgMLge.png)

![image](https://hackmd.io/_uploads/HJTi-gz8gl.png)

但後面想一想他也有用到特殊字元，再去看看有哪些可以用，有 python3

![image](https://hackmd.io/_uploads/BkiMGefIxx.png)

看起來沒有啥特殊字元 ( ">" 、& etc...)

![image](https://hackmd.io/_uploads/HkLrfezUlx.png)

成功! 

![image](https://hackmd.io/_uploads/H155zlG8ll.png)

進來之後找到一個 db ，但他有加密

![image](https://hackmd.io/_uploads/HyyQ4ez8ll.png)

![image](https://hackmd.io/_uploads/HkZKBxM8ee.png)


進來之後找到一個使用者叫做 `edward`，用 hydra 去爆破看看

![image](https://hackmd.io/_uploads/SyHrQeM8gg.png)

這邊想不到想法去看 wp ，發現他上傳 `./linpeas` 去看資料

這邊上傳 `linpeas` 的方式跟以前不太一樣，雖然一樣都是在 attack server 上面架了短暫的 python server，因為這邊沒有 `wget`，所以用 `curl` 試試看

![image](https://hackmd.io/_uploads/r1Xh8gMIgx.png)

成功! 

在這邊撈到了 local admin 的密碼跟帳號

![image](https://hackmd.io/_uploads/SkhqvxfLex.png)

這邊我用猜說 Zeno 已經被改名成 edward ，所以才沒辦法成功登入，然後也撈到了 `restaurant system` 的 admin pwd

![image](https://hackmd.io/_uploads/H1b0wefUxl.png)

![image](https://hackmd.io/_uploads/HyJydxf8xl.png)

> Get Flag1 : THM{070cab2c9dc622e5d25c0709f6cb0510}

let's 提權，`sudo ` 大法看到 reboot 

![image](https://hackmd.io/_uploads/ByP7dxMIlx.png)

suid 看起來也沒啥東西

![image](https://hackmd.io/_uploads/Hk7pYlM8xg.png)

但在 `/home/edward/.ssh` 有看到一個 `authorized_keys`

![image](https://hackmd.io/_uploads/Sk5eclzUle.png)

決定再用 `./linPeas` 看一次，因為 `/home/edward/.ssh` 權限不夠，於是跑去 `dev/shm` 載

![image](https://hackmd.io/_uploads/rkHKcezIxg.png)

使用之後看到 `/etc/systemd/system/zeno-monitoring.service` 可以讓我們有寫入的權限，嘗試用這個提權

![image](https://hackmd.io/_uploads/Hkm96eG8ll.png)


把它改成開機的時候會把 `/bin/bash` 加上 suid 

![image](https://hackmd.io/_uploads/r1GfAez8ee.png)


接著重新開機

![image](https://hackmd.io/_uploads/HJV2lZfIee.png)

連上之後用 `GTFOBins` 上的這坨

![image](https://hackmd.io/_uploads/BkV7WWGIlg.png)

Get Root!

![image](https://hackmd.io/_uploads/HJjQ--fUel.png)

> Get root Flag : THM{b187ce4b85232599ca72708ebde71791}

## Pwned!

![image](https://hackmd.io/_uploads/S1R8Z-zUgx.png)
