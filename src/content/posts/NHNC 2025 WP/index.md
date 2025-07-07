---
title: NHNC 2025 Web dkri3c1_love_cat Wrties Up
published: 2025-07-07
tags: [CTF]
category: Cyber Security
draft: false
image: 'image.png'
---

## Before

This is my first time writing English Writes up , apologize my poor English xDD

Story began after  TVE Joint College Entrance Examination ended , [LemonTea]( https://blog-lemontea.pages.dev/) ask me if I want to create a challenge to NHNC.

Hence , the gueesy Challenge borned :D .

## dkri3c1_love_cat

![image](https://hackmd.io/_uploads/Hyrt_gdSgl.png)

Actually, this challenge is very guessy xD , the main reason is that I want my Challenge won't be solved easily by AI 

OK,Let's talk about solution , when you connect to the website , you will find that you can use parameter `img` to read file

![image](https://hackmd.io/_uploads/SykSteOBxl.png)

![image](https://hackmd.io/_uploads/HJHDYx_Blg.png)

OK, And we try if it have the LFI with payload `../`

It will return 500 Internal Server Error

![image](https://hackmd.io/_uploads/HJd5tldSll.png)

Hence, we can guess this challenge is to read file with LFI , after a lot of guessy , you can use `....//` to get last directory , and our target is find where the flag file is.

![image](https://hackmd.io/_uploads/B1oVieuSgl.png)

according to the hint , we need to find the directory includeing `app.py`, just fuzzing the path , and you will find `....//....//app.py` will get the source code

![image](https://hackmd.io/_uploads/SyQy2xOrlg.png)

try to use the path `....//....//flag.txt` and you will get flag!

![image](https://hackmd.io/_uploads/BkfM3gdrge.png)

> flag:NHNC{dkri3c1_Like_Cat_oUo_>_<_c8763} 

unintended solve is that I find someone readfile with absolute path , if you are guessy king , you can guess that the directory name is app and flag is in the flag.txt

![image](https://hackmd.io/_uploads/BJ1_hxuHxx.png)
