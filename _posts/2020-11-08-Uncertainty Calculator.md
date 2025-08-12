---
title: Uncertainty Calculator
description: 用Qt和C++做一個不確定度計算機吧
author: cwz
date: 2020-11-08 11:40:00 +0800
last_modified_at: 2020-12-05 08:00:00 +0800
categories: [Software Development, Qt]
tags: [Software Development, Qt, C++]
img_path: assets/img/Uncertainty%20Calculator/
---

搬運自很久以前的部落格

## 專案連結

原始碼：[https://github.com/C-W-Z/UncertaintyCalculator](https://github.com/C-W-Z/UncertaintyCalculator)

版本下載：[https://github.com/C-W-Z/UncertaintyCalculator/releases](https://github.com/C-W-Z/UncertaintyCalculator/releases)

## 前言

花了約一天的時間，從下載Qt，然後摸索如何使用，最後我做出了一個小程式軟體。

沒錯，這是我的第一個Qt程式！不是"Hello World"哦！

當初為什麼會想做這個程式呢？

其實是因為高中做實驗有時候數據太多，用小算盤或計算機算不知道要算多久，所以就做了這個程式啦！雖然只是很簡單的東西，但是很實用呢。

## 軟體介紹

給大家看一下，下載之後會是這個樣子：

![Uncertainty Calculator v1.3](/assets/img/Uncertainty%20Calculator/1.PNG)
_註：UC 是 Uncertainty Calculator 的縮寫_

給大家看一下，下載之後會是這個樣子：

![測量不確定度Calculator v1.1](/assets/img/Uncertainty%20Calculator/2.PNG)
_打開之後就像這樣子(這是已經輸入數據的)(這是windows7的界面)_

![關於](/assets/img/Uncertainty%20Calculator/3.PNG){: .left }

用法也非常簡單，輸入數據，然後按確定就好了！

｢小數點後幾位｣則是看你要顯示幾位數的計算結果，預設是10，最高30位數。

但是記住，<span class="clr-red">算出來的數字只有15位數是絕對精準的！</span>(因為是用double儲存數字，計算時會丟失精度)

我是依照高中課本的算法算的，所以B類不確定度我直接用你輸入的｢儀器最小單位｣除以2根號3。

然後<span class="clr-red">每個數據的單位注意要一致</span>，例如上面那張圖中，1、2、3、4、5，以及0.1的單位都是相同的，cm或是mm或是m等等。簡單來說就是你輸入的數字要單位換算過啦！

還有就是，<span class="clr-red">這個程式不會幫你計算有效位數</span>，你要自己取哦~畢竟只是單純的計算程式嘛，就像電腦預設的計算機(小算盤)一樣。

<p class="txt-r clr-gray"><em>2020/11/8</em></p>

要執行程式有兩種方法：

一是直接點兩下就會直接執行了。

二是點右鍵，把它解壓縮(如果你的電腦有安裝Winrar的話)，接著會出現一個叫｢測量不確定度Calculator v1.1｣資料夾，找到裡面的｢測量不確定度Calculator.exe｣點兩下就可以了(<span class="clr-gold">請不要移除資料夾裡的其他檔案，也不要把資料夾裡的exe檔移動到其他地方</span>)。

<p class="txt-r clr-gray"><em>更新於 2020/11/9</em></p>

v1.0 和 v1.1的版本，圖示會是下面這樣，不過我覺得"Evaluation of measurement uncertainty Calculator"這個名字太長了，我打算在之後的版本改叫"Uncertainty Calculator"就好了，也就是UC。

![測量不確定度Calculator v1.1](/assets/img/Uncertainty%20Calculator/4.PNG){: .left }
_註：EC是Evaluation of measurement uncertainty Calculator的簡寫。_

｢測量不確定度Calculator｣也會改成英文｢Uncertainty Calculator｣

然後之後將會新增加減乘除的功能，預告一下。

<p class="clr-gray"><em>更新於 2020/11/10</em></p>

<p class="clr-red"><strong>重大更新！！！</strong></p>

1.3版本新增了不確定度的加減乘除運算，以後不用再自己用小算盤慢慢照著公式按啦！

我用了逆向波蘭表示法哦，呼~寫得超累的，目前主要的程式碼的部分已經有500行了呢，一大半都是在弄這個逆向波蘭，這次的更新裡，可以使用的運算符有：+-*/()，目前只有這些，但是以高中來講已經夠啦，像是次方什麼的公式，課本沒有講，我上網查也是眾說紛紜，所以目前只能夠用X*X*X*X*X*X這樣的東西來算了，總之我覺得嘛，這次做的真的很不錯呢，除了使用說明，我真的是懶得打，反正應該都會用吧？

![Uncertainty Calculator v1.3](/assets/img/Uncertainty%20Calculator/5.PNG){: .left }

<< 最後附上一張圖

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

其實，如果你想把它當成一般的計算機來用也是完全沒問題的(笑哭)

<p class="clr-gray"><em>更新於 2020/11/21</em></p>
