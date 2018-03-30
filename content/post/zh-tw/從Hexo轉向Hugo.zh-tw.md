---
title: "從Hexo轉向Hugo"
date: 2018-03-27T19:51:34-05:00
lastmod: 2018-03-29
tags: ["Hexo", "Hugo"]
categories: ["Dev"]
---

## 為什麼使用Hugo

一開始我選擇了Hexo做為我寫blog的工具,很大原因是我覺得Hexo的Next滿好看的,而且也有支援Github Page, build的速度感覺應該不會輸給Jekyll, 那時候雖然有考慮Hugo, 不過因為我對golang沒有經驗, 所以一開始還是沒有選擇它。

不過後來我便後悔了，因為我想讓blog有中英兩種語言, 而我用Hexo成功的方式是將原本的資料夾完整複製一份, 然後新增一個資料夾在原本的blog資料夾並將複製的資料夾貼上。 這做法我覺得很蠢, 因為這樣變成是每當我新增一篇文章我必須要在兩邊的資料夾內都先build一遍 

而且不知道為什麼我一開始使用hexo的next主題時預設的語言就是英文, 我新增中文網站後還是無法順利使用i18n讓他抓到Next裡language資料夾對應的語言檔, google的結果跟官網的docs也沒有很清楚的說明

基於以上種種原因, 我便跳到了Hugo

## 安裝Hugo

我的電腦環境Windows 10, 如果是Mac的人請參照官網的doc教學

至於Windows的安裝方法很簡單，先google搜尋hugo release

然後找到對印自己的作業系統版本與32或64位元

下載下來並解壓縮放在想要的資料夾路徑，我則是參照官網教學放在了<code>C:\Hugo</code>

{{< figure src="/img/hugo_path.jpg">}}

接著請到電腦的環境變數裡面的PATH新增<code>C:\Hugo\bin</code>

新增後在cmd底下輸入

    hugo version

若沒有什麼問題便代表正確安裝完成了~

## 安裝Hugo Theme

新增一個blog的方式如下

    hugo init blog

新增完後需要安裝一個theme才能正常啟動server, 沒安裝theme直接啟動server的話會沒有任何畫面

而我選擇的theme是一個叫[jane](https://github.com/xianmin/hugo-theme-jane)的主題


## 設定檔

由於我想要做中文跟英文雙語的blog, 所以我首先便在<code>/theme/jane/i18n</code>底下新增<code>zh-tw.yaml</code>檔案, 然後在<code>/blog/config.toml</code>的menu部分修改成

    [Languages.zh-tw]
      languageCode = "zh-tw"
      weight = 1
      title = "中文title"
    [Languages.en]
      languageCode = "en"
      weight = 2
      title = "English Title"

    /**
     * code block
    */

    [[Languages.en.menu.main]]             # config your menu              # 配置目录
      name = "Home"
      weight = 10
      identifier = "home"
      url = "/en"
    [[Languages.en.menu.main]]
      name = "Archives"
      weight = 20
      identifier = "archives"
      url = "/en/post/"
    [[Languages.en.menu.main]]
      name = "Tags"
      weight = 30
      identifier = "tags"
      url = "/en/tags/"
    [[Languages.en.menu.main]]
      name = "Categories"
      weight = 40
      identifier = "categories"
      url = "/en/categories/"
    [[Languages.en.menu.main]]
      name = "中文"
      weight = 50
      identifier = "中文"
      url = "/"

    [[Languages.zh-tw.menu.main]]             # config your menu              # 配置目录
      name = "首頁"
      weight = 10
      identifier = "home"
      url = "/"
    [[Languages.zh-tw.menu.main]]
      name = "歸檔"
      weight = 20
      identifier = "archives"
      url = "/zh-tw/post/"
    [[Languages.zh-tw.menu.main]]
      name = "標籤"
      weight = 30
      identifier = "tags"
      url = "/zh-tw/tags/"
    [[Languages.zh-tw.menu.main]]
      name = "分類"
      weight = 40
      identifier = "categories"
      url = "/zh-tw/categories/"
    [[Languages.zh-tw.menu.main]]
      name = "English"
      weight = 50
      identifier = "English"
      url = "/en"

以上是有關<code>config.toml</code>的設定。 接著在<code>/content/post</code>裡新增<code>en</code>與<code>zh-tw</code>資料夾，然後新增文章時檔案的命名方式變成<code>title.LanguageCode.md</code>

## 最後

不知道是因為theme的關係還是我有動到哪裡的設定, 中文首頁的頁面layout似乎有問題, 這點可能還要看如何修改

{{< figure src="/img/postscreenshot.jpg">}}


以後在再加上font-awesome的圖案跟自製css