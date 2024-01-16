---
title: 花札遊戲製作
description: 關於我做了一個花札網頁遊戲這檔事
author: cwz
date: 2023-07-24 12:00:00 +0800
last_modified_at: 2023-07-26 12:00:00 +0800
categories: [遊戲製作, Web]
tags: [遊戲製作, Web, HTML5, CSS, JavaScript] # TAG names should always be lowercase
---

搬運自原部落格

關於我做了一個花札網頁遊戲這檔事

## 遊戲連結

遊戲本體：[https://c-w-z.github.io/hanafuda/](https://c-w-z.github.io/hanafuda/)

原始碼：[https://github.com/C-W-Z/hanafuda](https://github.com/C-W-Z/hanafuda)

## 前言

這個暑假我心血來潮，突然想要做一個花牌遊戲，一開始想說用比較熟悉的Unity做，但我覺得一個牌類遊戲應該沒有很複雜，用Unity做的話檔案會很大有點浪費，而且要做成電腦還是手機的應用程式是個問題，於是後來我就研究了一下HTML的Canvas，決定把這個遊戲做在網頁上，這樣要玩就很方便了。

## 遊戲介紹

其實我把整個遊戲的規則、玩法和介紹都放在github上的[README](https://github.com/C-W-Z/hanafuda#readme)了，我自認寫的滿清楚，沒玩過的人應該也看得懂，如果看不懂，玩個一二局再看一次應該就懂了，我當初也是玩了幾局之後才慢慢了解規則的。

## 製作歷程

重點來了，這篇我不是要介紹花牌怎麼玩，而是講製作這個花牌網頁遊戲的過程。

簡單劃分整個遊戲，可以分為：

- 載入：網頁與資源的載入速度
- 畫面：你看到的遊戲畫面
- 機制：遊戲背後在跑的程式
- 互動：玩家點擊後會發生什麼

### 畫面

其中，畫面的部分我採用了有限狀態機，在不同的狀態就在Canvas上畫不同的畫面或動畫，例如主畫面和遊戲過程顯然是不同的狀態，另外每個動畫也被我單獨劃自成一個狀態，動畫結束才會切換到下一個狀態。

有點怕之後自己看不懂，紀錄一下自己畫動畫的程式：

```js
let startTime = null;
let time_func = new Function();
let next_func = new Function();
time_func = next_func = null;
function animate(time) {
    if (!startTime) // it's the first frame
        startTime = time || performance.now();
    if (time_func != null)
        time_func(time); // 執行time_func，一般是一幀的動畫(例如某張牌在一幀內往上移動多少之類的)
    redraw_canvas(); // 重畫整個畫面
    requestAnimationFrame(animate); // 下一幀
}
window.onload = function() { animate(startTime); }
```

其中2個動畫之間的連接是靠next_func，舉例來說，這是令一個動畫開始的程式

```js
startTime = performance.now();
time_func = 第一個動畫的函式;
next_func = 第二個動畫的函式;
```

而這是第一個動畫結束時要做的事情

```js
startTime = performance.now();
time_func = next_func;
next_func = null;
```

第二個動畫結束時則會`time_func = next_func = null`;

另外，有一個動畫中很常用到的東西叫做ease function，能讓動畫更順滑，有興趣可以參考[stack overflow上對ease function的介紹](https://stackoverflow.com/questions/8316882/what-is-an-easing-function)。

### 互動

實際上互動也與狀態有關，當玩家點擊時，我會判斷現在是什麼狀態，決定要做什麼反應，例如，現在是等待玩家選擇手牌的狀態，當玩家點擊，我就檢查玩家有沒有選到哪張手牌，如果有就進入下一個階段：選擇場牌，如果沒有就停留在這個狀態。

偵測滑鼠在canvas中的座標：

```js
function updateMouseXY(event) {
    let rect = event.target.getBoundingClientRect();
    mouse.x = (event.clientX - rect.left) / scaleRate;
    mouse.y = (event.clientY - rect.top ) / scaleRate;
}
```

會有scaleRate是因為canvas在網頁上的大小和實際解析度(畫素)不同，這是我設定canvas大小的code：

```js
R = window.devicePixelRatio;
// resolution of canvas
canvas.width = SCREEN_W * R;
canvas.height = SCREEN_H * R;
// auto adaptive the size of canvas by height / weight of the web page
scaleRate = Math.min(self.innerHeight / SCREEN_H, self.innerWidth / SCREEN_W);
canvas.style.width = SCREEN_W * scaleRate + 'px';
canvas.style.height = SCREEN_H * scaleRate + 'px';
```

### 機制

機制詳細展開就是：

- 主畫面與設定、統計等功能
- 遊戲過程
  - 決定親權(先手)的猜大小的小遊戲
  - 洗牌與發牌
  - 玩家選擇手牌、場牌、抽牌
  - 電腦出牌、抽牌
  - 檢查是否組成役與是否こいこい
  - 月結算與牌局結束
- 資料上傳、下載、儲存

這一部份有很多東西可以講，但我決定還是只說洗牌、畫畫面、電腦決策、資料上傳下載儲存這幾件事。

首先洗牌最簡單，有一個很有名的算法叫做Fisher-Yates演算法，原理很像selection sort的random版。

```js
for (let i = deck.length - 1; i > 0; i--) {
    const r = Math.floor(Math.random() * (i + 1));
    [deck[i], deck[r]] = [deck[r], deck[i]];
}
```

簡單來說就是把還沒排好部分中隨機抽一個出來放在排好的部分的最後，然後一直重複到全部排好，就洗完了。就是這麼簡單。

畫畫面其實也沒什麼好說的，重點在於減少draw的次數，提高效能。另外如果有旋轉、放大縮小等等，比起用相反的函式回復原本的狀態，或者是用`context.save()`和`context.restore()`，有個更好的選擇是用`context.setTransform(1, 0, 0, 1, 0, 0)`。

接下來是電腦決策，這應該是我整個遊戲做的最差的部分，畢竟我自己也不算花牌高手，不知道該寫什麼樣的策略比較好。

遊戲開始前會有AI Level的選項，有Lv0~3，其中Lv0和Lv1都是隨機出牌，Lv2和Lv3是按照牌的價值決定出什麼牌。

Lv2和Lv3最大的不同在於，Lv2是按照光、種、短冊、滓4種牌分價值高低，而Lv3則是每張牌的價值分的更細。Lv0和Lv1的不同點則在於，兩者對牌差的控制不同。

由於花牌是每回合出一張牌抽一張牌，等於有近乎一半的運氣成分，所以我寫了一個控制牌差的程式，其中Lv1的是雙方最多只能相差2張牌，而Lv0的是電腦不能比玩家的牌還多，也就是超簡單模式。

寫到這裡，你可能還不知道為什麼我覺得這部分做的最差，其實是因為，我有讓電腦互打幾百場，Lv2：Lv1的勝率是80%，Lv3：Lv2的勝率是60%，但問題是Lv3：Lv1的勝率只有70%。所以實在很糟糕。

最後，講到資料上傳、下載和儲存。

先講判斷物件格式是否相同的方法，不過這方法沒辦法判斷物件中的物件，以及順序問題(因為我沒有考慮那些)。

```js
function isArray(obj) {
    /* https://ithelp.ithome.com.tw/articles/10219475 */
    if (typeof Array.isArray === "function")
        return Array.isArray(obj); // 如果瀏覽器支援就用 isArray() 方法
    else  // 否則就使用 toString 方法
        return (Object.prototype.toString.call(obj) === "[object Array]");
}
function compareArrFormat(a, b) {
    if (a.length != b.length)
        return false;
    for (let i = 0; i < a.length; i++) {
        const ia = isArray(a[i]);
        const ib = isArray(b[i]);
        if (ia != ib)
            return false;
        if (ia && !compareArrFormat(a[i], b[i]))
            return false
    }
    return true;
}
/* compare whether two objs have same format */
function equalObjFormat(o1, o2) {
    const a = Object.entries(o1);
    const b = Object.entries(o2);
    if (a.length != b.length)
        return false;
    for (let i = 0; i < a.length; i++)
        if (a[i][0] != b[i][0])
            return false;
    return compareArrFormat(a, b)
}
```

舉例來說：a和b格式相同，但a、c、d格式都不同。

```js
a = {x: 0, y: [1, 2]};
b = {x: 5, y: [3, 4]};
c = {z: 0, y: [1, 2]};
d = {x: 0, y: [1000]};
```

儲存其實沒什麼好說的，就是localStorage，我是用一個class Data存資料，然後存取就是：

```js
class Data {
    constructor() {
        this.init();
        const obj = JSON.parse(localStorage.getItem('Data'));
        if (obj && equalObjFormat(this, obj)) // 判斷localStorage中的資料是否存在且格式正確
            Object.assign(this, obj);
        else this.store();
    }
    store() { localStorage.setItem('Data', JSON.stringify(this)); }
    init () {/* do something */}
}
```

最特別的是上傳和下載，這涉及到Promise、async、await等語法，我也是上網找才知道怎麼寫的。

```js
/* https://stackoverflow.com/questions/19721439/download-json-object-as-a-file-from-browser */
function downloadObjectAsJson(exportObj, exportName) {
	const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(exportObj));
	const downloadAnchorNode = document.createElement('a');
	downloadAnchorNode.setAttribute("href", dataStr);
	downloadAnchorNode.setAttribute("download", exportName + ".json");
	document.body.appendChild(downloadAnchorNode); // required for firefox
	downloadAnchorNode.click();
	downloadAnchorNode.remove();
}
/* https://stackoverflow.com/questions/36127648/uploading-a-json-file-and-using-it */
const getJsonUpload = () =>
    new Promise(resolve => {
        const inputFileElement = document.createElement('input');
        inputFileElement.setAttribute('type', 'file');
        inputFileElement.setAttribute('multiple', 'false'); // 只能上傳單個檔案
        inputFileElement.setAttribute('accept', '.json');
        inputFileElement.addEventListener(
            'change',
            async (event) => {
                const { files } = event.target;
                if (!files) return;
                const filePromises = [...files].map(file => file.text());
                resolve(await Promise.all(filePromises));
            },
            false,
        );
        inputFileElement.click();
    });
/* upload function */
async function uploadData() {
    const jsonFiles = await getJsonUpload();
    let obj;
    try {
        obj = JSON.parse(jsonFiles[0]);
    } catch (error) {
        alert('This is not a JSON file');
        return;
    }
    if (equalObjFormat(data, obj)) {
        Object.assign(data, obj);
        data.store();
        alert('Upload Sucess');
    } else alert('Data Format Error!');
}
/* download function */
function downloadData() { downloadObjectAsJson(data, 'Data'); }
```

### 載入

最後，資源載入，這是一件感覺很不起眼但其實非常重要的事情，因為這對遊戲體驗影響很大，尤其我是把網頁掛在github pages上，而它的限制就是快取只有10分鐘，也就是關掉網頁10分鐘後重開，就要重新把資源下載一次(上一次下載的已經被刪掉了)，如果載入速度太慢就會很討厭。

網路上有很多可以測試載入速度的網站，最有名的應該是Google的[PageSpeed Insights](https://pagespeed.web.dev/)，沒事可以自己測測看自己的網站。

這邊講一下我用的加快資源載入的方法。

首先是圖片：用一個陣列把圖片名按重要程度排序記起來，可以先載入比較重要的圖片。

其中執行到`img.src = "path of image";`的時候會載入圖片。

```js
const LOAD_ORDER = [2, 3, 1, ...];
const IMG_NUM = LOAD_ORDER.length;
let img = new Array(IMG_NUM);
for (let i = 0; i < IMG_NUM; i++) {
    img[LOAD_ORDER[i]] = new Image();
    img[LOAD_ORDER[i]].onload = onImgLoad;
    img[LOAD_ORDER[i]].src = `${LOAD_ORDER[i]}.webp`;
}
function onImgLoad() {/* do something */}
```

然後是html中載入css和js檔的順序，可以用preconnect、preload和defer等語法調整不同檔案的載入順序

以下是我的寫法，不過不保證這樣一定是載入最快的。

```html
<head>
    <!-- Preload & Preconnect -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link rel="preload" as="style" href="style.css">
    <link rel="preload" as="script" href="main.js">
    <!-- Load CSS -->
    <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=某某字體&display=swap">
    <link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
    ...
    <!-- Load Scripts -->
    <script src="main.js" defer></script>
    <script src="other.js" defer></script>
</body>
```

preconnect是告訴瀏覽器你等一下要跟哪個網站連接(如果是字體則要加上crossorigin，否則它會下載2次)，preload則是告訴瀏覽器哪些檔案是一定要先載完的。defer則是讓瀏覽器載入scripts時不會打斷渲染(背景載入)，同時由上到下按照順序執行scripts。

以上面的例子，瀏覽器會先下載Google字體和style.css、main.js，然後渲染body中的元素，再執行main.js，最後才下載並執行other.js。

順道一題，現在這個網站我沒有調整載入順序(主要是有點麻煩)，所以速度不怎麼樣，不過目前網站還沒有圖片，所以應該也不至於載太久啦。