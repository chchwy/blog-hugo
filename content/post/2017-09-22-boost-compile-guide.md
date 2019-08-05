---
date: 2017-09-22
title: Boost 極簡編譯法
isCJKLanguage: true
tags: [Boost, C++]
---

最近因為工作的緣故需要編譯 `Boost`。Boost 這套大名鼎鼎的 C++ Library 中，大多數的模組都是 header-only，意思是模組裡只有標頭檔(`*.hpp`) 沒有實現檔(`*.cpp`)，所以不需要編譯，引入(#include)標頭檔就可以直接用了。只有少部份模組需要先編譯，這裡紀錄一下編譯 Boost 的方法。

環境: Windows 10 編譯器: Visual Studio 2015


* 第一步，雙擊 Boost 根目錄下的 bootstrap.bat，產生 Boost 自帶的編譯工具 b2.exe 和 bjam.exe
* 第二步，用 b2 來編譯 boost，指令如下：

```
b2 toolset=msvc-14.0 address-model=64 --with-system
```

* 我並沒有深究 b2 和 bjam 到底有什麼差異，我用 b2
* 編譯的參數中，`toolset` 指編譯器，msvc-14.0 就是 VS2015，msvc-11.0 就是 VS2012，gcc 就是 gcc。
* `address-model` 指定 32 / 64 bit
* `--with-xxx` 指定要編譯的模組名稱，例如要編譯 system 就打 `--with-system`，編譯 chrono 就是 `--with-chrono`
* 或者用 `-a` 要求編譯全部模組。編譯全部模組需要不少時間，我編了 15 分鐘發現還沒完成就放棄了。
* 產出的 lib 檔默認放在 **stage/lib** 目錄下，瞄一眼裡面的檔案，如果看見 `libboost_chrono-vc140-mt-1_61.lib` 之類的檔案冒出來就是編譯成功了。以該檔名為例，可以得知我們成功編譯了 chrono 模組，適用編譯器 vc140，boost 版本 1.61。

接著把把 boost 的根目錄加進 Include Path，把 /stage/lib 加進 Library Search Path，應該就可以順利使用我們自己編譯的 boost 了。不需要一一指名每個用到的 lib 檔，挺方便的。