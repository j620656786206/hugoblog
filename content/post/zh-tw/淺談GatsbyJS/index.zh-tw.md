---
title: "淺談GatsbyJS"
date: 2018-04-09T21:16:37-05:00
tags: ["GatsbyJS", "React"]
categories: ["網站開發"]
thumbnail: "gatsby_logo.jpg"
---

前陣子在逛medium的時候偶然看到了<a href="https://www.gatsbyjs.org/" class="gatsbyjs">GatsbyJS</a>, 大概了解了一下後, 知道<a href="https://www.gatsbyjs.org/" class="gatsbyjs">GatsbyJS</a>是用React框架做的一個static site generator, 原本正在煩惱我是否又要從Hugo跳到<a href="https://www.gatsbyjs.org/" class="gatsbyjs">GatsbyJS</a>, 但研究了一下後, 我還是繼續選擇了Hugo

<!--more-->

# 什麼是GatsbyJS?

官網上介紹<a href="https://www.gatsbyjs.org/" class="gatsbyjs">GatsbyJS</a>是一個靜態的PWA (Progressive Web App) 產生器, <a href="https://www.gatsbyjs.org/" class="gatsbyjs">GatsbyJS</a>只會載入需要的html, css, data, javascript, 讓網頁的讀取速度變快, 而且網頁讀取後, <a href="https://www.gatsbyjs.org/" class="gatsbyjs">GatsbyJS</a>會預先載入其他頁面所需的資源, 所以在整個網站的瀏覽上會讓人感覺很快

# 我需要GatsbyJS嗎?

瀏覽了一下官網提供的範例網站後, 確實讓人覺得速度很快, 但讓我考慮的點如下:

* 如果要使用GatsbyJS的話, 一堆的plugin跟設定就都要自己來, 而且可能還要學GraphQL

* 而且Gatsby也不像Hugo一樣有別人做好的theme可以直接套用, 所以等於一切都要自己從頭來

* 網站的生成速度不知如何, 目前看人家build的速度大概都要2,3秒, 不知道當網站的scale增加後的速度會有多快

所以基於以上原因, Hugo目前還是我所偏好的, 但未來要是GatsbyJS有成長的趨勢的話, 將blog轉變成一個React App或許也是一個不錯的選擇, 只是目前我沒有什麼api需要fectch, 也沒有什麼data需要load,單純寫寫blog用markdown寫寫應該就足夠了

# 參考文章

* Hugo vs GatsbyJS: https://goo.gl/Tk3Z7n

* GatsbyJS介紹: https://goo.gl/H1c8Cz
