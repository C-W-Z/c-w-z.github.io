---
title: 反應遊戲CIRCLES
description: 關於我做了一個反應網頁遊戲這檔事
author: cwz
date: 2023-07-28 12:00:00 +0800
last_modified_at: 2023-07-30 12:00:00 +0800
categories: [遊戲開發, Web]
tags: [遊戲開發, Web, HTML5, CSS, TypeScript]
---

搬運自原部落格

關於我做了一個反應網頁遊戲這檔事

## 遊戲連結

遊戲本體：[https://c-w-z.github.io/circles/](https://c-w-z.github.io/circles/)

原始碼：[https://github.com/C-W-Z/circles](https://github.com/C-W-Z/circles)

## 前言

之所以會想做這個遊戲，主要是因為曾經玩過一款叫做Madobu的遊戲，他還有個前作叫Madosa，但是好像已經從Play商店下架了。這兩個遊戲玩法幾乎一樣，基本上和我這個遊戲的EASY模式差不多。

剛好最近比較無聊，就做了一個和Madobu有點像但更加複雜(更難)的網頁遊戲，順便學一點TypeScript，至於為什麼叫CIRCLES，單純只是因為這遊戲裡有很多圓而已XD。

7/28的我：出乎意料之外，原本預計要3天的時間，結果竟然2天就做完了。

7/29的我：結果又發現一些小bug，而且在手機上測試會卡，到最後還是做了3天。

## 遊戲介紹

玩法很簡單，選擇難度之後立即開始，當白色的的球碰到大圓上白色的線時點擊/按任意鍵就可以消除所有碰到線的球，同時得分。如果球移動超過線但你沒有反應過來，或者是你在球碰到線之前就點擊/按任意鍵的話，遊戲就會結束。我個人建議是用電腦按鍵玩，會比較順暢而且可以用好幾隻手指。

每個難度都有好幾個階段，基本上會越來越難，INSANE模式到後面根本不是人玩的，我看電腦auto只覺得太瘋狂了。

## 製作歷程

這個遊戲我是用TypeScript寫的，大家似乎都很推TypeScript(跟原生JavaScript比較的話)，就來學學看。寫完這個遊戲之後，我感覺TypeScript其實也沒有很難(也可能只是我寫的遊戲太簡單？)，只是比較麻煩，它對各種情況都很嚴謹，例如說如果`let a:number|null=null`，你就不能直接做和a有關的運算，而是要在前面加上`if (a!=null)`的判斷(當然這個例子可以直接用`Number(a)`，但如果是更複雜的type就不行了)。

和花牌遊戲比起來，我這次用了3層canvas，下面那層畫大圓和線，平常只要更新中間那層的小球位置就行了，可以減少一點資源的使用，第三層放到後面說。之後我可能會把花牌遊戲也更新成多個canvas，其實那時候我就有嘗試了，但一直做不出來，主要是canvas重疊的部分，現在剛好紀錄一下要怎麼做：

```html
<body>
    <div class="canvases">
        <canvas id="canvas1"></canvas>
        <canvas id="canvas2"></canvas>
    </div>
</body>
```

```css
.canvases { position: relative; }
canvas {
    position: absolute;
    left: 0;
    top: 0;
}
```

真的意外的非常簡單，只要這樣2個canvas就會重疊在一起，而且會隨著`.canvases`的位置移動，頂多再在`.canvases`和`canvas`都加個width和height標籤。

另外，像這種畫圓的，把原點設在中心點會真的會簡單很多。即`context?.setTransform(1, 0, 0, 1, canvas.width/2, canvas.height/2)`或`context?.translate(canvas.width/2, canvas.height/2)`。

整個遊戲比較可惜的是生成小球的code，因為我是直接暴力random生成，不符合條件(例如靠太近)就重生一遍，導致我測試的時候遇到過幾次無窮迴圈整個卡住(因為360度塞不下了，下一顆球不管生在哪都不符合條件)，後來改成如果重來超過10次就停下才解決，但是這只是治標而已，以後有時間應該改寫一下算法，讓它能夠一次就生成符合條件又夠隨機的小球們。

還有一點，就是我原本有做小球之間的碰撞，想說同一圈的每個小球方向也可以隨機，然後會互相碰撞，那個函式我沒有刪掉，但是考慮到那樣的話遊戲也未免太複雜了，會整個眼花撩亂，所以就沒有啟用。

我原本還想說其他部分都沒什麼好講的，結果遇到了bug之後我決定還是紀錄一下解決方法。雖然這應該也不算bug，是流暢度問題，即我用手機測試時會有一點卡。

解決方法就是不要每一顆球都`fill()`或`stroke()`，在同一幀之內，一次把所有小球的路徑都走一遍，然後再一次性`fill()`。

```js
function drawPath(x:number, y:number, r:number) {
    if (!context) return;
    context.moveTo(x, y);
    context.arc(x, y, r, 0, 2 * Math.PI);
    context.closePath(); // close the subpath created after moveTo(x, y)
}
function animate(time:number) {
    if (context) {
        context.clearRect(-canvas.width/2, -canvas.height/2, canvas.width, canvas.height);
        context.beginPath();
        for (const b of balls)
            b.drawPath(b.x(time), b.y(time), b.radius);
        context.fillStyle = 'white';
        context.fill();
    }
    requestAnimationFrame(animate);
}
```

不過從上面可以看出，這只適用於顏色相同的情況，如果顏色不同，就只能單獨一個一個畫，這就是第3層canvas的用處了，即用來畫小球淡出和變紅的動畫。

7/30更新：沒想到手機還有觸控問題，就是手指點擊會同時觸發`ontouchstart`和`onmousedown`，真的超級不合理的啦，搞了很久還是沒有解決辦法，我只能偵測如果有觸控功能就把`onmousedown`的偵測關掉，但是有個問題在於如果有人是用觸控筆電，就無法用滑鼠玩了(但還是可以用鍵盤)。
