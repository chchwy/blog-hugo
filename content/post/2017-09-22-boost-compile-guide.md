---
date: 2017-09-22
title: Boost 極簡編譯法
isCJKLanguage: true
tags: [ Boost, C++ ]
---

最近工作上需要用到 Boost 這套著名的 C++ Library。Boost 大多數的模組都是 header-only，但是依然有少數的模組需要先編譯才能使用。在這裡就紀錄一下編譯 Boost 的方法。

- 第一步，雙擊執行根目錄下的 bootstrap.bat，產生 Boost 自帶的編譯工具 b2.exe 和 bjam.exe
- 第二步，用 b2 來編譯 boost 指令如下：

```
    b2 toolset=msvc-14.0 address-model=64 --with-system
```

- 我並沒有深究 b2 和 bjam 到底有什麼差異，我個人就是用 b2
- 編譯的參數中，`toolset` 指編譯器，msvc-14.0 就是 VS2015，msvc-11.0 就是 VS2012，gcc 就是 gcc。
- `address-model` 指定 32 / 64 bit
- `--with-xxx` 就是指定要編譯的模組名稱，例如要編譯 system 就打 --with-system，編譯 chrono 就是 --with-chrono
- 或者可以用 `-a` 要求完整編譯全部模組，但是我在自己筆電上測試一下發現 15 分鐘還沒編完就放棄了。
- 產出的 lib 默認放在 **stage/lib** 目錄下，瞄一眼裡面的文件，如果有看見 `libboost_chrono-vc140-mt-1_61.lib` 之類的檔案冒出來就是編譯成功了。以前面那個檔名為例，就是成功編譯了 chrono 模組，適用編譯器是 vc140，boost 版本 1.61。

把 boost 的根目錄加進 Include Path，然後把 stage/lib 加進 Library Search Path，專案應該就可以使用 boost 了。不需要一一指名每個用到的 lib 檔，算是挺方便的。