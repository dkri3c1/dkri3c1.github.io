---
title: AIS3 學員 & AIS3 Junior 助教 2025 心得
published: 2025-08-25
tags: [AIS3]
category: Cyber Security
draft: false
image: 'good.png'
---


# AIS3

## 前言

今年 Pre-Exam 後兩天跟高中同學跑去小琉球爽 (小琉球真的好漂亮，偷偷附個風景照)

![image](https://hackmd.io/_uploads/H136mQstxx.png)


![image](https://hackmd.io/_uploads/rkV0QQsKel.png)

所以排名差不多在 130 幾，怕自己進不去所以就順便投了甄選，結果最後上了 XDD，然後組別的話選了**軟體及IoT安全**

![Screenshot 2025-08-10 at 11.19.00 AM](https://hackmd.io/_uploads/HyG31cHdeg.png)

![Screenshot 2025-08-10 at 11.13.54 AM](https://hackmd.io/_uploads/Hk0dRYrOle.png)

然後因為去年睡外套很痛苦的關係，今年就帶了一個可樂果小枕頭+棉被過來，7 天的痛苦程度從 80 變成 40 了，愛可樂果，然後這邊主要會是講專題的部分，因為我課程得部分其實都忘記了 xDD

![Screenshot 2025-08-10 at 11.33.22 AM](https://hackmd.io/_uploads/Bk0WXcHugx.png)



## Day 0 

這一次的隊友有 [CX330](https://blog.cx330.tw/) & [Jackoha](https://jackoha.github.io/) & dd(抱歉我不知道要怎麼 tag)，然後群佬除我QAQ

這一次來的目標很明確，就是想要多學一點技術，所以和隊友討論之後決定在上課的時候不碰專題(除了資安倫理宣導)，原本的目標是決定挖 0Day ，啊沒有挖到就去鬼轉刻一個工具，至於挖的方向一開始是想說以 Binary 跟 Web 組去做分類，然後去挖開源專案．


## Day1 

課程的部分主要就是馬老師的 Windows Kernel 的東西(超硬)，這邊埋個坑是未來會補在這邊學到的筆記，直接跳轉到晚上討論專題的部分，CX 老大哥突然說我們可以去挖 Router 的 0 Day ，之後就挑了 totolink 這樣



## Day2

經過 CX 老大哥的挑燈夜戰之後，他找到了一個 Cmdi 的洞，結果助教看到我們的洞發現那個洞水到不行，所以就叫我們換一台 Router 去挖，然後從這邊開始我和 dd 就在架環境了 XD


## Day3

又過了一個晚上，CX 老大哥又挖到一個 Netis Router 的 Cmdi ，不是我想說三小，這人 0 Day 就跟路邊隨便撿的一樣，隨便看就有了，真的太屌了www，之後我就開始和 dd 一起搞這垃圾環境

## Day4 

在 Day3 的半夜因為太餓了跑去吃泡麵，結果吃完之後隔天爆拉一頓= =，但是因為泡泡麵的時候想到我們可以不用執著在用 qemu 本身去跑，我們也用用看其他的軟體去模擬，最後用 firmAE 跑起來了，所以我最後用了 10 分鐘解決我們卡了至少 3 天的問題= =，早上來的時候就成功把環境架出來了!!之後就是交給 jackoha 寫一個 RCE 的 script 了，然後真的非常感謝助教們幫我們解決要如何串好一個 Attack Chain 的問題www

## Day 5 

今天大致上就是 Jackoha 在修 Exploit ，把他寫成 RCE 進去之後把主要頁面改成 vincent55 的好駭客圖片+ 上 Pwned By 我們這組的所有組員這樣，半夜的時候敲了助教來幫我們看一下簡報跟 re 稿(這邊稍微跟隊友說個抱歉，我想說隨便丟個梗圖，結果很沒料QQ)，然後助教就突然說我們可以去看看可不可以  auth bypassed ，因為在一開始的 requests 上沒有 cookie ，之後因為太累了所以就直接跑去睡了，結果早上看訊息的時候發現 jackoha 說可以 auth bypassed ，最後就從 CVSS 8.2 的洞變成 CVSS 9.3 :D  
![Screenshot 2025-08-10 at 12.02.13 PM](https://hackmd.io/_uploads/SkWAFcr_eg.png)

## Day 6

報告當天的好事接連不斷xDD，就在報告前幾個小時的時候， dd 把我們之前用 qemu 直接跑不起來的問題解決了，簡報的部分變的更完整了，現在有點想不起來最後專題發表的事情了www，但大致上就是我有點太緊張，所以講比較快，然後最後有一個結論的部分我不小心忘記我要講啥，結果我直接丟給 CX ，然後他就當場講出一坨很有意涵的 summary ，CX 老哥真的太扛了 Orz

## 結語

最後在頒獎的時候聽到台上的人喊到我們組別的時候，整個人興奮到快跳起來XDD，從來沒有想過這一輩子在 AIS3 可以拿到最佳專題，非常感謝我的組員們都這麼 Carry，在環境架不起來的時候可以給那麼多的鼓勵

最後的最後，在這邊埋一個坑就是以後可能會做一個關於我們回報這個漏洞的一些細節 uWu

![Screenshot 2025-08-10 at 2.49.28 PM](https://hackmd.io/_uploads/rJ7-bpSuex.png)



# AIS3 Junior

## Junior 心得

由於助教的感悟沒有當 AIS3 學員來ㄉ多，所以我就簡短帶過XDD，

今年依舊跑來當助教: D，雖然沒有做專題的壓力，但我覺得要把對於資訊沒有任何基礎的學生從無到有教會真的很難，真的很敬佩其他組的助教，畢竟我們這邊有兩個去 AIS3 ㄉ炸魚仔，甚至 Pre-Exam 都比我前面= =。

真心覺得台南的物價好便宜，然後食物都好好吃ㄛXDD，感謝在地人 [Dr.Dog](https://asia-hokak.github.io/) ，讓我們完美ㄉ度過ㄌ這幾天，這邊大大ㄉ跟你們說一聲對不起，那幾天不知道為啥 11 12. 就想上床睡覺ㄌ，沒辦法陪你們一起捲 T_T

最後覺得很可惜的是在第二天晚上，因為颱風接近台南，所以提早一天上完課就放人走ㄌ，沒辦法彌補我太早睡ㄉ過錯QQ，希望以後有機會的話可以補償你們T_T

最後ㄉ最後放上一張 AIS3 軟3ㄉ團聚照片>_<

![image](https://hackmd.io/_uploads/rJchSe9Kxx.png)



