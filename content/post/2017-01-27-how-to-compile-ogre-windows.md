---
date: 2017-04-02
title: Ogre 2.1 編譯指南 (Windows篇)
draft: true
isCJKLanguage: true
---
# Ogre 2.1 編譯指南 (Windows篇)
老牌的開源 3D 渲染引擎 Ogre3D 最新的 2.1 版，除了視覺提昇，效能也變快。但是不知道為什麼，目前官網上只抓的到 1.9 版。想用 2.1 版的人得要自己下載源碼編譯。本篇文章就是紀錄我編譯 Ogre 2.1 的步驟。順帶一題，這是 Windows 平台的編譯步驟，之後會另寫一篇新文章講 iOS 平台。

## 下載 Source code

Ogre3D 編譯需要的 Repo 位於兩處，分別是 1. Ogre3D 引擎本體以及 2. Ogre 第三方函式庫

```
https://bitbucket.org/cabalistic/ogredeps
https://bitbucket.org/sinbad/ogre
```

Ogre3D 用的是一個比較少見的版本控制系統 hg，如果不熟悉這套系統可以直接下載原始碼zip包就好了，記得要抓 `v2-1` 這個分支。

## CMake

Ogre3D 使用 CMake 當作專案建構工具。這是我第一次用 CMake，花了一些時間才搞懂用法。用的是 [CMake 3.7.0][0]。CMake 本身不能編譯專案，但是可以在不同的平台上產生該平台的本地專案，我用的是 Windows ，所以就指定 CMake 產生 Visual Studio 2015 的專案。像在 Mac 上就是產生 Xcode 專案 。

[0]: https://cmake.org/  "CMake official site"

## 編譯依賴庫  ogredeps

編譯引擎本體之前，要先來編譯第三方依賴庫。首先打開 `CMake-gui`，CMake 的標準起手式，第一是指定源碼所在，第二是自動產生的專案位置。我的設定如下:

[ogredeps](/img/cmake-ogredeps.png)

第一欄指向 ogredeps 的根目錄，該目錄下有個叫做 `CMakeList.txt`的檔案就沒錯了。第二欄則是指定所有臨時產生的檔案要放哪，包括 Visual Studio 的專案檔以及編譯完成後的 DLL 檔等等。按照慣例是在根目錄下創建一個名叫 build 的子目錄。


設定好之後，按下 Configure，選擇編譯器版本 `Visual Studio 14 2015 Win64`，再按下 Generate ，然後按下 Open Project 應該就會打開 Visual Studio ，可以開始編程式了。

**很重要**記得一點，除了跑 ALL_BUILD 之外，還要一次 INSTALL 專案，debug/release 都要，這樣外部依賴庫的編譯才算完成。

## 編譯 Ogre3D 引擎

跟剛剛下載外部依賴庫一樣，Ogre 源碼倉庫位於 [https://bitbucket.org/sinbad/ogre](https://bitbucket.org/sinbad/ogre)


    hg clone  C:/OgreSDK/Ogre -r v2-1

剛抓下來時的代碼是在 default 分支，是 1.10 版本的代碼，而非我們想要的最新版 2.1，所以要自己手動切換到 `v2-1` branch。

## 六、編譯 Ogre

跟編譯外部依賴庫一樣的步驟，打開 CMake GUI，指定源碼所在和編譯產出的位置，按下 Configure。

我的路徑設定:

https://d2mxuefqeaa7sj.cloudfront.net/s_85E8565F5205F5539CA001E4B030E59660CECE48B2BFA44493ADA6D770AD88F5_1482899221973_snipaste20161228_152650.png


按下 Configure 按鈕之後呢，應該會跳出一個錯誤訊息。但是別擔心，這是因為 Ogre 不知道外部依賴庫的位置，所以要做一些設定，我已經框出需要設定的地方：

https://d2mxuefqeaa7sj.cloudfront.net/s_85E8565F5205F5539CA001E4B030E59660CECE48B2BFA44493ADA6D770AD88F5_1482899456764_snipaste20161228_152956.png

- Paging、RTShaderSystem、Terrain 這三個模組還沒跟上 2.1 版的腳步，不能通過編譯。
- OGRE_BUILD_SAMPLES2 是官方提供的 2.1 版範例集。
- OGRE_DEPENDENCIES_DIR 是外部依賴庫的位置，注意路徑是在 build 目錄的下一層。
- OGRE_SIMD_SSE2 勾起來會產生 Linking error，原因不明。

都設定好之後，再按 Congifure & Generate，應該就可以順利產生 Ogre.sln 並編譯了。

下圖是 Ogre3D 的 Forwrad3D 範例：

https://d2mxuefqeaa7sj.cloudfront.net/s_85E8565F5205F5539CA001E4B030E59660CECE48B2BFA44493ADA6D770AD88F5_1482903694425_+2016-12-28+16.38.56.png


