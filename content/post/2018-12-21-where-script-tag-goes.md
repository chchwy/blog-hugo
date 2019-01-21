---
date: 2018-12-21
title: "<script> 標籤應該放在哪裡？"
isCJKLanguage: true
tags: [Web]
draft: true
---

HTML 裡頭的 `<script>` 到底應該要放哪兒？

最基本的用法是把 `<script>` 標籤放在 `<head>` 區塊裡面。但是最近研讀別人的前端代碼的時候，我發現有許多不同的位置，就研究了一下。

經典用法 (放在 head 區塊裡)：

```html
<html>
<head>
  <script src="path-to-my.js"></script>
  <script src="path-to-another.js"></script>
</head>
...
```

經典用法並沒有什麼大問題。但是隨著現代 javascript 函式庫越來越肥，一些問題漸漸浮現出來。

原來瀏覽器解析 HTML 是照著順序由上往下解析的。而當瀏覽器遇到 `<script>` 標籤的時候，必須暫時停下手頭的工作，停止建構 DOM ，暫緩渲染網頁，把控制權交給 javascript 虛擬機，直到 .js 檔案全部都下載執行完畢之後，才會繼續渲染網頁。

於是又大又肥的 js 函式庫就成了網頁渲染的瓶頸。使用者可能得要瞪著空白的頁面，等待好幾秒鐘才能看見網頁。因為眾多 .js 檔案聚集在接近 HTML 文件開頭的地方，在等待 js 腳本下載跟執行完畢之前，瀏覽器是沒辦法開始繪製頁面的。

### 傳統解法

既然網頁是照著順序由上往下解析的，那麼只要把 `<script>` 標籤

### 現代解法

現代解法就是給 `script` 標籤加上 sync 或者 defer 修飾符。

https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script
https://stackoverflow.com/questions/436411/where-should-i-put-script-tags-in-html-markup
