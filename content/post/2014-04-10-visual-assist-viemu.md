---
date: "2014-02-14"
title: "解決 ViEmu 與 Visual Assist X 共存的衝突"
tags: ["Visual Studio"]
isCJKLanguage: true
---

在 Visual Studio 2013 上同時安裝兩大神器: ViEmu 以及 Visual Assist X 時會導致自動補完的功能癱瘓。只安裝其中任一個都沒事，兩個同時裝時才會出事。

在 Visual Assist 的上發現一條有用的資訊:
[Compatibility with ViEmu](http://support.wholetomato.com/default.asp?W271)

把路徑中的 VANet10 換成 VANet12 ，照著指示去修改註冊表就行了。
`HKEY_CURRENT_USER\Software\Whole Tomato\Visual Assist X\VANet12\TrackCaretVisibility`

把這個值從 01 改成 00 ，記得修改之前要先關閉 Visual Studio 否則會沒用。

