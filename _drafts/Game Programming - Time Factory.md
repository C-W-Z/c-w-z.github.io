---
title: Game Programming - Time Factory
description: 台大資工遊戲設計修課紀錄
author: cwz
# date: 2024-01-16 20:23:19 +0800
# last_modified_at: 2024-01-16 21:18:17 +0800
categories: [遊戲製作, Unity]
tags: [遊戲製作, Unity, CSharp] # TAG names should always be lowercase
# published: false
# hidden: true
---

我在台大資工的系選修課 -- 遊戲設計 Game Programming的修課紀錄。



我們的素材大部分是來自itch.io，少部分是我和文乂畫的，然後音樂和特效是qmao做的。

程式是所有人(主要是我)，關卡設計是Slimlix和文乂。

一個系統(e.g. Dialogue System, Player Control)的Script最好交給一個人做就好，可以減少看別人Code的時間，然後再把不同的系統組合起來(每個系統會有一些Public Function和Getter作為API)。

檔名拼錯不是不能改，但要溝通好。

你注意到的細節，別人不一定會注意到，所以注意到就直接說出來，不要以為別人一定會知道。

我們是除了master之外每個人一個branch，在自己的branch上做事然後再Merge到master上。

可能需要一個人負責Pull Request，管理所有的Commit會更好，但說實話Unity的Scene(`.unity`)檔案每次Merge的時候如果Conflit根本看不出來Conflit的到底是什麼地方，所以每個Scene同時只能有一個人動。

Playtest很有用，人數越多越好，越符合目標客群越好，也可以向專業人士詢問意見。
