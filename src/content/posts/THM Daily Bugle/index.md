---
title: THM Daily Bugle
published: 2025-07-10
tags: [THM]
category: Cyber Security
draft: false
image: 'image.jpg'
---

## Before 

最近開始練滲透，會多很多滲透文章的 wp UwU

## Recon

```bash=
┌──(kali㉿kali)-[~]
└─$ nmap -sC -sV -Pn 10.10.37.177
Starting Nmap 7.95 ( https://nmap.org ) at 2025-07-02 11:34 EDT
Nmap scan report for 10.10.37.177
Host is up (0.33s latency).
Not shown: 997 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey:
|   2048 68:ed:7b:19:7f:ed:14:e6:18:98:6d:c5:88:30:aa:e9 (RSA)
|   256 5c:d6:82:da:b2:19:e3:37:99:fb:96:82:08:70:ee:9d (ECDSA)
|_  256 d2:a9:75:cf:2f:1e:f5:44:4f:0b:13:c2:0f:d7:37:cc (ED25519)
80/tcp   open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.6.40)
| http-robots.txt: 15 disallowed entries
| /joomla/administrator/ /administrator/ /bin/ /cache/
| /cli/ /components/ /includes/ /installation/ /language/
|_/layouts/ /libraries/ /logs/ /modules/ /plugins/ /tmp/
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.6.40
|_http-generator: Joomla! - Open Source Content Management
|_http-title: Home
3306/tcp open  mysql   MariaDB 10.3.23 or earlier (unauthorized)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 40.62 seconds
```


## Exploit

有開 80 port ，網頁長這樣

![image](https://hackmd.io/_uploads/Sk6W40fBge.png)

一樣 gobuster 開掃 

```bash=
┌──(kali㉿kali)-[~]
└─$ gobuster dir -u 'http://10.10.37.177/' -w /usr/share/wordlists/dirb/big.txt -t 100
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.37.177/
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.htpasswd            (Status: 403) [Size: 211]
/.htaccess            (Status: 403) [Size: 211]
/administrator        (Status: 301) [Size: 242] [--> http://10.10.37.177/administrator/]
/bin                  (Status: 301) [Size: 232] [--> http://10.10.37.177/bin/]
/cache                (Status: 301) [Size: 234] [--> http://10.10.37.177/cache/]
/cgi-bin/             (Status: 403) [Size: 210]
/cli                  (Status: 301) [Size: 232] [--> http://10.10.37.177/cli/]
/components           (Status: 301) [Size: 239] [--> http://10.10.37.177/components/]
/images               (Status: 301) [Size: 235] [--> http://10.10.37.177/images/]
/includes             (Status: 301) [Size: 237] [--> http://10.10.37.177/includes/]
/language             (Status: 301) [Size: 237] [--> http://10.10.37.177/language/]
/layouts              (Status: 301) [Size: 236] [--> http://10.10.37.177/layouts/]
/libraries            (Status: 301) [Size: 238] [--> http://10.10.37.177/libraries/]
/media                (Status: 301) [Size: 234] [--> http://10.10.37.177/media/]
/modules              (Status: 301) [Size: 236] [--> http://10.10.37.177/modules/]
/plugins              (Status: 301) [Size: 236] [--> http://10.10.37.177/plugins/]
/robots.txt           (Status: 200) [Size: 836]
/templates            (Status: 301) [Size: 238] [--> http://10.10.37.177/templates/]
/tmp                  (Status: 301) [Size: 232] [--> http://10.10.37.177/tmp/]
Progress: 20469 / 20470 (100.00%)
===============================================================
Finished
===============================================================
```

有開 `/robots.txt`，看起來很有料

![image](https://hackmd.io/_uploads/ryAKNAzSex.png)

點進去 `/administrator` 這個路徑，發現這一套框架叫做 `joomla!`

![image](https://hackmd.io/_uploads/Hksd0z5Bgl.png)

msfconsole 看一下有沒有 exploit 可用，滿多的可以一個一個試試看

![image](https://hackmd.io/_uploads/B1hgJXqBxe.png)

用 `scanner/http/joomla_version` 得知以下資訊

![image](https://hackmd.io/_uploads/rybCV7cSgg.png)

去 google 找看看有沒有 joomla 3.7.0 的 exploit，找到這個

https://github.com/BaptisteContreras/CVE-2017-8917-Joomla

![image](https://hackmd.io/_uploads/S1m0_m5Bgg.png)

發現他把 username 跟經過 hash 的 password 都噴出來ㄌ，有料

目前已知 username 是 `jonah`，密碼的部分則是一坨 bcrypt 的東西，用 john 爆看看，喔然後久到我跑去看 wp 跟用 hashcat ...

![image](https://hackmd.io/_uploads/Bk7lb4cSlg.png)

密碼是 `spiderman123` 終於= =

登進去之後要想辦法 RCE，去找找看哪裡可以做到 file upload

![image](https://hackmd.io/_uploads/Hy-_-VcSeg.png)

參考 https://www.youtube.com/watch?v=8MHM9QXCI_A 在 extension 找到，把 error.php 改成 reversed shell

![image](https://hackmd.io/_uploads/H1L27VqBgx.png)

Get shell!

發現我們連進去普通使用者的權限都沒 

![image](https://hackmd.io/_uploads/BkXY445Bgl.png)

suid 看一下

![image](https://hackmd.io/_uploads/Byw9ENqrll.png)

看起來都不能用，去看看能不能找到 jjameson 的帳號，在 `/var/www/html/configuration.php` 找到一坨密碼

![image](https://hackmd.io/_uploads/r1z784cSge.png)

經過一堆嘗試之後，我們找到密碼 `nv5uz9r3ZEDzVjNu`

![image](https://hackmd.io/_uploads/ByPuLN9Blg.png)


> Get Flag1: 27a260fe3cba712cfdedb1c86d80442e

接下來提權的局 ， sudo -l 看一下

```bash=
[jjameson@dailybugle ~]$ sudo -l
Matching Defaults entries for jjameson on dailybugle:
    !visiblepw, always_set_home, match_group_by_gid, always_query_group_plugin, env_reset, env_keep="COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS", env_keep+="MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS
    LC_CTYPE", env_keep+="LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES", env_keep+="LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE", env_keep+="LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET
    XAUTHORITY", secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User jjameson may run the following commands on dailybugle:
    (ALL) NOPASSWD: /usr/bin/yum
```

![image](https://hackmd.io/_uploads/rJeC8Ncrll.png)

yum 可以用，直接用下面這一坨就可以拿到 root

![image](https://hackmd.io/_uploads/r1h1vE9rlx.png)

> GetFlag2 : eec3d53292b1821868266858d7fa6f79



## Pwned

![image](https://hackmd.io/_uploads/HJKmPVcSgx.png)
