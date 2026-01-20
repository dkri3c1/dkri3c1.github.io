---
title: HTB Sea
published: 2026-01-20
tags: [HTB,Linux]
category: PenTest
draft: false
image: 'image.png'
---

## 前言

學期終於結束了，休息完之後繼續扁靶機，現在的平台從 Tryhackme 轉到 HacktheBox ㄌ，只能說難度完全不是一個等級，繼續加油ㄅQQ

## Recon
```
┌──(kali😺dkri3c1)-[~/Downloads/dist]
└─$ sudo rustscan -a 10.129.1.214 -r 1-65535 --ulimit 5000 -- -sC -sV -Pn 
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Port scanning: Making networking exciting since... whenever.

[~] The config file is expected to be at "/root/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.129.1.214:22
Open 10.129.1.214:80
[~] Starting Script(s)
[>] Running script "nmap -vvv -p {{port}} -{{ipversion}} {{ip}} -sC -sV -Pn" on ip 10.129.1.214
Depending on the complexity of the script, results may take some time to appear.
[~] Starting Nmap 7.95 ( https://nmap.org ) at 2026-01-17 03:52 EST
NSE: Loaded 157 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 03:52
Completed NSE at 03:52, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 03:52
Completed NSE at 03:52, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 03:52
Completed NSE at 03:52, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 03:52
Completed Parallel DNS resolution of 1 host. at 03:52, 0.02s elapsed
DNS resolution of 1 IPs took 0.03s. Mode: Async [#: 3, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating SYN Stealth Scan at 03:52
Scanning 10.129.1.214 [2 ports]
Discovered open port 80/tcp on 10.129.1.214
Discovered open port 22/tcp on 10.129.1.214
Completed SYN Stealth Scan at 03:52, 0.11s elapsed (2 total ports)
Initiating Service scan at 03:52
Scanning 2 services on 10.129.1.214
Completed Service scan at 03:52, 6.25s elapsed (2 services on 1 host)
NSE: Script scanning 10.129.1.214.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 03:52
Completed NSE at 03:52, 2.91s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 03:52
Completed NSE at 03:52, 0.40s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 03:52
Completed NSE at 03:52, 0.00s elapsed
Nmap scan report for 10.129.1.214
Host is up, received user-set (0.075s latency).
Scanned at 2026-01-17 03:52:36 EST for 10s

PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 63 OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 e3:54:e0:72:20:3c:01:42:93:d1:66:9d:90:0c:ab:e8 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCZDkHH698ON6uxM3eFCVttoRXc1PMUSj8hDaiwlDlii0p8K8+6UOqhJno4Iti+VlIcHEc2THRsyhFdWAygICYaNoPsJ0nhkZsLkFyu/lmW7frIwINgdNXJOLnVSMWEdBWvVU7owy+9jpdm4AHAj6mu8vcPiuJ39YwBInzuCEhbNPncrgvXB1J4dEsQQAO4+KVH+QZ5ZCVm1pjXTjsFcStBtakBMykgReUX9GQJ9Y2D2XcqVyLPxrT98rYy+n5fV5OE7+J9aiUHccdZVngsGC1CXbbCT2jBRByxEMn+Hl+GI/r6Wi0IEbSY4mdesq8IHBmzw1T24A74SLrPYS9UDGSxEdB5rU6P3t91rOR3CvWQ1pdCZwkwC4S+kT35v32L8TH08Sw4Iiq806D6L2sUNORrhKBa5jQ7kGsjygTf0uahQ+g9GNTFkjLspjtTlZbJZCWsz2v0hG+fzDfKEpfC55/FhD5EDbwGKRfuL/YnZUPzywsheq1H7F0xTRTdr4w0At8=
|   256 f3:24:4b:08:aa:51:9d:56:15:3d:67:56:74:7c:20:38 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBMMoxImb/cXq07mVspMdCWkVQUTq96f6rKz6j5qFBfFnBkdjc07QzVuwhYZ61PX1Dm/PsAKW0VJfw/mctYsMwjM=
|   256 30:b1:05:c6:41:50:ff:22:a3:7f:41:06:0e:67:fd:50 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHuXW9Vi0myIh6MhZ28W8FeJo0FRKNduQvcSzUAkWw7z
80/tcp open  http    syn-ack ttl 63 Apache httpd 2.4.41 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: Sea - Home
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 03:52
Completed NSE at 03:52, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 03:52
Completed NSE at 03:52, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 03:52
Completed NSE at 03:52, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 10.49 seconds
           Raw packets sent: 2 (88B) | Rcvd: 2 (88B)

```
## Exploit

在 `how_to_parcipate` 路徑下發現 domain name: `sea.htb`

![image](https://hackmd.io/_uploads/BktsbROB-e.png)

用 feroxbuster 去掃目錄

```bash=
┌──(kali😺dkri3c1)-[~/Downloads/dist]
└─$ feroxbuster -u "http://sea.htb" -x .php
 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.13.1
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://sea.htb/
 🚩  In-Scope Url          │ sea.htb
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.13.1
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 💲  Extensions            │ [php]
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET       84l      209w     3341c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
403      GET        7l       20w      199c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
301      GET        7l       20w      231c http://sea.htb/plugins => http://sea.htb/plugins/
301      GET        7l       20w      230c http://sea.htb/themes => http://sea.htb/themes/
301      GET        7l       20w      228c http://sea.htb/data => http://sea.htb/data/
200      GET      118l      226w     2731c http://sea.htb/contact.php
301      GET        7l       20w      232c http://sea.htb/messages => http://sea.htb/messages/
301      GET        7l       20w      234c http://sea.htb/data/files => http://sea.htb/data/files/
200      GET       85l      260w     3681c http://sea.htb/messages/how-to-participate
404      GET       66l      150w     3341c http://sea.htb/messages/fc
[##>-----------------] - 42s    38436/360038  6m      found:8       errors:334    
🚨 Caught ctrl+c 🚨 saving scan state to ferox-http_sea_htb_-1768641413.state ...
[##>-----------------] - 42s    38441/360038  6m      found:8       errors:334    
[##>-----------------] - 42s     7970/60000   189/s   http://sea.htb/ 
[##>-----------------] - 42s     7816/60000   188/s   http://sea.htb/themes/ 
[#>------------------] - 42s     5268/60000   127/s   http://sea.htb/plugins/ 
[##>-----------------] - 41s     6804/60000   164/s   http://sea.htb/data/ 
[##>-----------------] - 37s     7066/60000   190/s   http://sea.htb/messages/ 
```

繼續對 `plugins` 跟 `themes` 做掃描

```bash=
┌──(kali😺dkri3c1)-[~]
└─$ feroxbuster -u "http://sea.htb/themes" -x .php

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.13.1
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://sea.htb/themes
 🚩  In-Scope Url          │ sea.htb
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.13.1
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 💲  Extensions            │ [php]
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET       84l      209w     3341c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
403      GET        7l       20w      199c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
301      GET        7l       20w      230c http://sea.htb/themes => http://sea.htb/themes/
301      GET        7l       20w      235c http://sea.htb/themes/bike => http://sea.htb/themes/bike/
301      GET        7l       20w      239c http://sea.htb/themes/bike/css => http://sea.htb/themes/bike/css/
301      GET        7l       20w      239c http://sea.htb/themes/bike/img => http://sea.htb/themes/bike/img/
500      GET        9l       15w      227c http://sea.htb/themes/bike/theme.php
404      GET       29l       62w     3341c http://sea.htb/themes/bike/logout
404      GET       29l       62w     3341c http://sea.htb/themes/bike/img/scripts
404      GET       29l       62w     3341c http://sea.htb/themes/bike/css/modules
404      GET       29l       62w     3341c http://sea.htb/themes/bike/china.php
404      GET       29l       62w     3341c http://sea.htb/themes/produtos
404      GET       29l       62w     3341c http://sea.htb/themes/bike/css/cron.php
404      GET       29l       62w     3341c http://sea.htb/themes/bike/moderation.php
404      GET       64l      143w     3341c http://sea.htb/themes/bike/ntopic
200      GET       85l      260w     3681c http://sea.htb/themes/bike/css/how-to-participate
404      GET       29l       62w     3341c http://sea.htb/themes/bike/UK
404      GET       29l       62w     3341c http://sea.htb/themes/bike/bible.php
404      GET       64l      143w     3341c http://sea.htb/themes/bike/css/forgot-password
404      GET       29l       62w     3341c http://sea.htb/themes/bike/creative.php
200      GET        1l        1w        6c http://sea.htb/themes/bike/version
404      GET       29l       62w     3341c http://sea.htb/themes/bike/css/slides.php
200      GET       85l      260w     3681c http://sea.htb/themes/bike/img/how-to-participate
404      GET       29l       62w     3341c http://sea.htb/themes/JScripts.php
404      GET       64l      143w     3341c http://sea.htb/themes/bike/img/upfiles
200      GET       21l      168w     1067c http://sea.htb/themes/bike/LICENSE
404      GET       29l       62w     3341c http://sea.htb/themes/bike/best-mortgages.php
404      GET       29l       62w     3341c http://sea.htb/themes/bike/img/vcard
404      GET       29l       62w     3341c http://sea.htb/themes/bike/img/deportes.php
404      GET       29l       62w     3341c http://sea.htb/themes/bike/css/pdf_extract.php
404      GET       29l       62w     3341c http://sea.htb/themes/bike/ram.php
404      GET       29l       62w     3341c http://sea.htb/themes/bike/css/ygptemp.php
404      GET       29l       62w     3341c http://sea.htb/themes/bike/img/cancer.php
404      GET       29l       62w     3341c http://sea.htb/themes/bike/pp_repository
404      GET       29l       62w     3341c http://sea.htb/themes/bike/autonews
200      GET       85l      260w     3681c http://sea.htb/themes/how-to-participate
404      GET       29l       62w     3341c http://sea.htb/themes/help2
404      GET       64l      143w     3341c http://sea.htb/themes/it-it
404      GET       29l       62w     3341c http://sea.htb/themes/nuovosito
404      GET       64l      143w     3341c http://sea.htb/themes/flash-download.php
200      GET        1l        9w       66c http://sea.htb/themes/bike/summary
404      GET       64l      143w     3341c http://sea.htb/themes/bike/img/mydata
404      GET       29l       62w     3341c http://sea.htb/themes/bike/img/celeb.php
404      GET       29l       62w     3341c http://sea.htb/themes/funcions
404      GET       29l       62w     3341c http://sea.htb/themes/bike/verification.php
404      GET       29l       62w     3341c http://sea.htb/themes/bike/css/_clients
404      GET       29l       62w     3341c http://sea.htb/themes/bike/css/abonnement
404      GET       29l       62w     3341c http://sea.htb/themes/bike/css/sfa.php
404      GET       29l       62w     3341c http://sea.htb/themes/bike/kontakte
404      GET       29l       62w     3341c http://sea.htb/themes/bike/marques.php
404      GET       29l       62w     3341c http://sea.htb/themes/bike/menuimages.php
404      GET       29l       62w     3341c http://sea.htb/themes/bike/css/_scriptsGlobal
404      GET       29l       62w     3341c http://sea.htb/themes/bike/img/nancy
404      GET       64l      143w     3341c http://sea.htb/themes/bike/css/_scriptlibrary.php
404      GET       29l       62w     3341c http://sea.htb/themes/bike/img/secure1
404      GET       29l       62w     3341c http://sea.htb/themes/bike/valentines.php
404      GET       29l       62w     3341c http://sea.htb/themes/FORMgen
404      GET       29l       62w     3341c http://sea.htb/themes/bike/anchor
404      GET       29l       62w     3341c http://sea.htb/themes/bike/css/vdata
404      GET       29l       62w     3341c http://sea.htb/themes/bike/century
404      GET       29l       62w     3341c http://sea.htb/themes/bike/careerpath.php
404      GET       29l       62w     3341c http://sea.htb/themes/bike/css/poisk-po-sajtu
404      GET       29l       62w     3341c http://sea.htb/themes/bike/css/wrap
404      GET       29l       62w     3341c http://sea.htb/themes/bike/css/623
404      GET       29l       62w     3341c http://sea.htb/themes/bike/css/AllRecentChanges.php
404      GET       29l       62w     3341c http://sea.htb/themes/bike/css/RESOURCES.php
404      GET       29l       62w     3341c http://sea.htb/themes/tune
404      GET       29l       62w     3341c http://sea.htb/themes/uch
404      GET       29l       62w     3341c http://sea.htb/themes/bike/egrpo
404      GET       29l       62w     3341c http://sea.htb/themes/bike/emailsret
404      GET       64l      143w     3341c http://sea.htb/themes/1156.php
404      GET       29l       62w     3341c http://sea.htb/themes/bike/css/dropoff
404      GET       29l       62w     3341c http://sea.htb/themes/bike/img/mpsearch
404      GET       29l       62w     3341c http://sea.htb/themes/bike/pdfreports.php
404      GET       29l       62w     3341c http://sea.htb/themes/bike/css/naplok
404      GET       29l       62w     3341c http://sea.htb/themes/bike/radyo.php
404      GET       64l      143w     3341c http://sea.htb/themes/bike/css/sommer.php
[####################] - 6m    240088/240088  0s      found:75      errors:1304
[####################] - 5m     60000/60000   184/s   http://sea.htb/themes/
[####################] - 6m     60000/60000   178/s   http://sea.htb/themes/bike/
[####################] - 6m     60000/60000   178/s   http://sea.htb/themes/bike/img/
[####################] - 6m     60000/60000   178/s   http://sea.htb/themes/bike/css/   
```

```bash=
┌──(kali😺dkri3c1)-[~]
└─$ feroxbuster -u "http://sea.htb/plugins"

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.13.1
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://sea.htb/plugins
 🚩  In-Scope Url          │ sea.htb
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.13.1
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
403      GET        7l       20w      199c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
404      GET       84l      209w     3341c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
301      GET        7l       20w      231c http://sea.htb/plugins => http://sea.htb/plugins/
200      GET       85l      260w     3681c http://sea.htb/plugins/how-to-participate
404      GET       64l      143w     3341c http://sea.htb/plugins/document
404      GET       64l      143w     3341c http://sea.htb/plugins/2019
404      GET       29l       62w     3341c http://sea.htb/plugins/interfaces
404      GET       29l       62w     3341c http://sea.htb/plugins/Community-Care
404      GET       29l       62w     3341c http://sea.htb/plugins/business_cards
404      GET       29l       62w     3341c http://sea.htb/plugins/motorsports
404      GET       29l       62w     3341c http://sea.htb/plugins/mplayer
404      GET       29l       62w     3341c http://sea.htb/plugins/reindirizzato
[####################] - 3m     30009/30009   0s      found:10      errors:337
[####################] - 3m     30000/30000   144/s   http://sea.htb/plugins/  
```


發現 `plugins` 沒料，而根據 fuzzing 出來的路徑可以得到以下資訊

LICENSE:

![image](https://hackmd.io/_uploads/HyTCCDjrZe.png)


version:

![image](https://hackmd.io/_uploads/r1X1JdsSWe.png)

接下來把 MIT License 的內文丟到 google 會發現他跑出一個 CMS 的 Themes Project

https://github.com/turboblack/black/tree/master

![image](https://hackmd.io/_uploads/ByAry_irWe.png)

而根據目錄掃描的結果其實可以猜測 theme 的部分是 bike

https://github.com/robiso/bike

![image](https://hackmd.io/_uploads/SyDK1ujBWg.png)

這邊大概可以確定系統框架是 `WonderCMS`，然後 Version 是 `3.2.0`，去找看看有沒有適合的 POC

找到這篇:

https://gist.github.com/prodigiousMind/fc69a79629c4ba9ee88a7ad526043413

發現彈不成功，改用

https://github.com/Tea-On/CVE-2023-41425-RCE-WonderCMS-4.3.2

就成功了 = =，目前使用者是 www-data

![image](https://hackmd.io/_uploads/r1OacOjBbe.png)

在 `/home` 找到兩個使用者 `amay` & `geo`

![image](https://hackmd.io/_uploads/rJJ_Cdjrbx.png)


在 `/var/www/sea/data` 找到 `database.js`，並且發現一段看起來像是 bcrypt 的東西

![image](https://hackmd.io/_uploads/HJtojdsS-g.png)

這邊踩了一個坑是 database.js 因為語言關係，放`/`會用到 `\`，而這樣會影響 hashcat 導致無法成功，因此需要自己把 `\`給拿掉

![image](https://hackmd.io/_uploads/S1B5TuoH-g.png)

炸出來之後是 `mychemicalromance`

![image](https://hackmd.io/_uploads/SJPm0djH-e.png)


用 ssh 成功登入 `amay`

![image](https://hackmd.io/_uploads/HJj2AujBWx.png)

成功拿到 user flag: `23f.........`

![image](https://hackmd.io/_uploads/r184eFoSWx.png)

目前就沒想法了，於是去 wp 搜看看怎麼打提權

利用 `netstat -ano` 看一下本身還有開啥服務

![image](https://hackmd.io/_uploads/BkkieFiBZl.png)

發現 `port 8080` 有東西，於是利用 ssh 去做 port forwarding

```bash=
┌──(kali😺dkri3c1)-[~]
└─$ ssh -L 8888:localhost:8080 amay@10.129.2.85
```

輸入密碼之後在 kali 上面開 localhost:8080

![image](https://hackmd.io/_uploads/BkXu-FoBbx.png)

發現會訪問這個頁面，用 amay 的帳號跟密碼去登入

會進入這個頁面

![image](https://hackmd.io/_uploads/rJvnZtsSZe.png)

觀察 analyze log file 的部分，會發現他實際上會去執行一些指令

![image](https://hackmd.io/_uploads/Sk47MtjHZx.png)

用 burp 去做測試，攔截 analyze 部分的封包

![image](https://hackmd.io/_uploads/SJJ5mtjBbx.png)

嘗試把 log_file 改成 `/root/root.txt` 發現會被 WAF 擋掉

![image](https://hackmd.io/_uploads/rkG3mFiHbg.png)

用 URL encode 也是一樣

![image](https://hackmd.io/_uploads/r16TXFoHbl.png)

用 command injection 去截斷，發現 `/etc/passwd;ls` 在檔案後面接上 ; 就可以成功讀檔，

![image](https://hackmd.io/_uploads/rJVAVKjrWx.png)

於是就這樣拿到 root.txt: `f01a59a0.........`

![image](https://hackmd.io/_uploads/rJd7BtsrWx.png)


## Pwned

![image](https://hackmd.io/_uploads/B1yLHtiSWx.png)
