---
title: 用Jekyll模板製作個人網站
description: 關於我自己把個人網站搬到這個用Jekyll模板做的網站的經歷
author: cwz
date: 2024-01-16 13:15:33 +0800
last_modified_at: 2024-01-16 16:19:37 +0800
categories: [個人網站]
tags: [Jekyll, Chirpy]
---

關於我自己把[舊的個人網站](https://c-w-z.github.io/)搬到這個用Jekyll模板做的網站的經歷。

> 僅限在Windows 10上，其他OS我不能保證正確，且未來版本更新也可能會有所不同
{: .prompt-warning }

## 安裝Jekyll

有任何問題請參考[Jekyll官網的Doc](https://jekyllrb.com/docs/)

### 安裝[Ruby](https://www.ruby-lang.org/en/downloads/)

Windows按照它的建議用[RubyInstaller](https://rubyinstaller.org/downloads/)，版本也按照Ruby官網的建議(裝Ruby+Devkit的)，反正大於`2.5.0`就好。安裝過程中我應該是沒有動什麼選項，一切按照它建議的來。

用以下指令確定是否安裝成功(我一開始只能在`Start Command Prompt with Ruby`{: .filepath }之中執行，要等一段時間才能在一般的`cmd`{: .filepath }中執行，應該是環境變數的設置生效需要時間，或直接重開機應該也行？)

```shell
ruby -v
```

### 安裝[RubyGems](https://rubygems.org/pages/download)

這是用來管理Ruby的套件的東西，[RubyInstaller](https://rubyinstaller.org/)應該會幫你一起裝好。

用以下指令確定是否安裝成功(跟Ruby一樣，我一開始也只能在`Start Command Prompt with Ruby`{: .filepath }之中執行，要等一段時間才能在一般的`cmd`{: .filepath }中執行)

```shell
gem -v
```

### 安裝[GCC](https://gcc.gnu.org/install/)

Windows建議裝[MinGW](https://sourceforge.net/projects/mingw/)，或者說MinGW32。不過我很久以前就裝了，所以有問題就看網路上的教學吧。

裝好之後在環境變數的`Path`新增`C:\MinGW\bin`(要生效可能要重開機)。

```shell
gcc -v
g++ -v
```

用以下指令確定是否安裝成功

### 安裝[GNU Make](https://www.gnu.org/software/make/)

Windows就裝[Make for Windows](https://sourceforge.net/projects/gnuwin32/)，我同樣也是裝了一段時間的，所以有問題請爬文。

裝好之後在環境變數的`Path`新增`C:\Program Files (x86)\GnuWin32\bin`。

用以下指令確定是否安裝成功

```shell
make -v
```

### 安裝Jekyll

```shell
gem install jekyll bundler
```

裝好之後，測試一下可不可以用

用以下指令創建新的Jekyll Project(`<your website name>`可以替換成任何你想要的名字，但建議不要用中文或奇怪符號)

```shell
jekyll new <your website name>
```

然後進入那個資料夾

```shell
cd <your website name>
```

架起最基礎的網站

```shell
bundle exec jekyll serve # 或 bundle exec jekyll s
```

> 如果你用的是`Ruby v3.0.0`以上的版本，這一步可能會失敗，請先執行`bundle add webrick`，然後再試一次。
{: .prompt-danger }

然後你就可以在[http://localhost:4000](http://localhost:4000)或[http://127.0.0.1:4000](http://127.0.0.1:4000)看到它了！

如果你對Project做了任何更動，只要儲存(例如按下`Ctrl+S`)，Jekyll就會自動幫你重新編譯，只要重新載入就可以看見變更了。

想關掉該網頁只要回到剛才的cmd然後按下`Ctrl+C`即可。

## 使用模板

網路上應該很容易找到很多Jekyll的模板/主題。我自己是用[Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy)這個模板，並且是用它的[Chirpy Starter Template](https://github.com/cotes2020/chirpy-starter)。

這是[Chirpy的官方教學](https://chirpy.cotes.page/)，大部分步驟都寫得滿清楚的，但還是有一些東西沒寫到，我就寫在Q&A了。

關於Chirpy的markdown文章語法請見：[Text and Typography](https://github.com/cotes2020/jekyll-theme-chirpy/blob/master/_posts/2019-08-08-text-and-typography.md)

## Q&A

### 為什麼選擇Jekyll？

[Hugo](https://gohugo.io/)和[Hexo](https://hexo.io/)雖然也都是不錯的選擇，但是我不喜歡[Chocolatey](https://chocolatey.org/)。然而在Windows上用Hugo最簡單的方式就是裝Chocolatey；Hexo使用[Node.js](https://nodejs.org/)，也有可能會用到Chocolatey(裝Node.js時它會說有些套件需要Chocolatey才能用，建議安裝)。

### 為什麼選擇Chirpy？

很單純的原因：因為它很好看。

### 我是用它的Starter Template/從它的GitHub fork過來，那MIT License可以改嗎？

確切來說，不能改，但是可以(且應該)加上你自己的License，如果你決定使用MIT Lincense，只要加上一行即可。

```
MIT License

Copyright (c) 2024 Chen-Wei, Zhang # 我是加上這一行
Copyright (c) 2021 Cotes Chung
```
{: file='LICENSE' }

如果你想要使用GPL或其他LICENSE也可以，但要保留它原本的LICENSE。

> 註：GitHub Repo的License跟文章的License可以是不同的。
{: .prompt-info }

有不懂得請爬文，我自己也是看stackoverflow上的文章(但其實不保證我是對的)。

### 如果我想改文章預設的在頁尾的MIT License怎麼做？頁尾的`Using the Chirpy theme for Jekyll.`怎麼弄掉？

要改`_data/locales/[lang].yml`(沒有就去[Chirpy的GitHub](https://github.com/cotes2020/jekyll-theme-chirpy)下載)，其中`[lang]`是`_config.yml`中`lang`的值(預設是`en`)

### 左邊的Sidebar我想要不只有`Categories`，`Tags`，`Archives`，`About`這四個Page怎麼辦？

在`_tabs/`底下新增一個`<page name>.md`，`<page name>`是你想要的名字，Front Matter參考`_tabs/about.md`。

同樣，如果想要把已有的page刪掉，只要把`_tabs/`底下對應的markdown檔案刪掉再重新編譯即可。

### 為什麼我的文章沒有顯示Last Modified Time？

因為那個功能來自一個插件，你應該可以看到`_plugins\posts-lastmod-hook.rb`，但可惜的是hook在GitHub Pages上不起作用。

解決辦法：手動在每個post的Front Matter中加上`last_modified_at: YYYY-MM-DD HH:MM:SS +/-TTTT`。

### 為什麼我GitHub Action會Build失敗？

我自己遇到的問題是文章中包含了`Http`連結，然後它就跟我說

```
Run bundle exec htmlproofer _site \
...
For the Links check, the following failures were found:
...
http://some_http_link is not an HTTPS link
```

原因是有個叫[HTML Proofer](https://github.com/gjtorikian/html-proofer)的東西會幫你檢查網頁的各種問題。

只要在`.github\workflows\pages-deploy.yml`中加上你要的[HTML Proofer 參數](https://github.com/gjtorikian/html-proofer?tab=readme-ov-file#configuration)即可。

```yml
run: |
    bundle exec htmlproofer _site \
    \-\-disable-external=true \
    \-\-enforce_https=false \ # 我自己是加上這一行
    \-\-ignore-urls "/^http:\/\/127.0.0.1/,/^http:\/\/0.0.0.0/,/^http:\/\/localhost/"
```
{: file='.github\workflows\pages-deploy.yml' }

### 怎麼讓所有的外部連結都在新分頁開啟？

方法一：手動在每個`[description](link)`後面都加上`{: target="_blank" }`。

方法二：安裝[Jekyll Target Blank插件](https://github.com/keithmifsud/jekyll-target-blank)，安裝方法它的[GitHub頁面](https://github.com/keithmifsud/jekyll-target-blank)有說。

### 如何設定SEO？

請見[Jekyll SEO Tag](https://github.com/jekyll/jekyll-seo-tag)。

我還參考了[改善 SEO 的幾種方法](https://ktinglee.github.io/how-improve-jekyll-seo/)。

使用[Google Search Console](https://search.google.com/search-console/)和[Google Rich Results Test](https://search.google.com/test/rich-results)。

### 如何使用自己想要的字體？

[Chirpy官網的教學](https://chirpy.cotes.page/posts/getting-started/#customizing-stylesheet)其實有寫，剩下的就是怎麼寫SCSS了，這是語法問題，不過我發現大部分CSS的語法在SCSS都可以用(至少目前沒發現不能用的)。

至於Chirpy產生的HTML element有哪些class和id，這個我是直接到網站上去用Chrome的開發人員工具(按`F12`)看的。

### 你用的是什麼字體？怎麼這麼好看

中文我用的是[霞鶩文楷](https://github.com/lxgw/LxgwWenKai)，英文是很多種[Google Font](https://fonts.google.com/)，例如[Caudex](https://fonts.google.com/specimen/Caudex)和[EB Garamond](https://fonts.google.com/specimen/EB+Garamond)。
