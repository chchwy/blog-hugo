---
date: 2017-04-02
title: Ogre 2.1 編譯指南 (Windows篇)
draft: true
---
# Ogre 2.1 編譯指南 (Windows篇)
本文紀錄我在 Windows 平台編譯 Ogre 2.1 版的步驟。老牌 3D 渲染引擎 Ogre 的 2.1 版，主要的賣點是支援的 PBS (Physically based rendering)以及效能大幅提昇。但是同時也因為架構翻新，所以網路上的大多教學都過時了。

- 版本控制系統 hg
- ![0000276530](\Pictures\0000276530.JPG)
- cmake
- 下載 & 編譯 Ogre 外部依賴庫
- 下載 & 編譯 Ogre 引擎

## 一、安裝 CMake

CMake 是一套跨平台的專案建置工具，去 [CMake 官網](https://cmake.org/download/) 抓最新版來安裝。寫這篇文章的當下是 3.7.1 版。

## 二、安裝 hg

Ogre 使用的源碼版本控制系統是 hg (沒想到在這個幾乎已經被git一統天下的年代還可以看到 hg)。Windows 上常見的 hg 客戶端有 [TortoiseHG](http://tortoisehg.org/) 和 [SourceTree](https://www.sourcetreeapp.com/) ，自己撿一套來裝就可以了。

## 三、下載 Ogre 的外部依賴庫

除了引擎本體以外，Ogre 還使用了一系列的第三方庫，像是 FreeType 字體渲染，FreeImage 讀取圖檔等等。Ogre 官方很貼心的幫我們把這些第三方庫都收集在一起：
[https://bitbucket.org/cabalistic/ogredeps](https://bitbucket.org/cabalistic/ogredeps)，用 hg 把代碼倉庫抓下來。


    hg clone https://bitbucket.org/cabalistic/ogredeps C:/OgreSDK/ogredeps


## 四、編譯外部依賴庫

這是我第一次使用 CMake，所以花了一點時間才搞懂用法。基本上 CMake 本身不能編譯專案，而是會依據不同的平台來產生該平台上的原生專案，我用的是 Visual Studio 2015，所以指定 CMake 產生 VS2015 的專案出來。

操作 CMake 的起手式是指定兩個路徑位置，第一個是源碼所在，第二是編譯的產出路徑。

把第一欄指向剛剛抓下的 Ogre 外部依賴庫的根目錄，確認一下該目錄下有個檔案叫做 `CMakeList.txt` 就沒錯了。第二欄則是指定編譯完的 lib 檔的位置，依照慣例是建一個名叫 build 的子目錄。

我的設定如下:

https://d2mxuefqeaa7sj.cloudfront.net/s_85E8565F5205F5539CA001E4B030E59660CECE48B2BFA44493ADA6D770AD88F5_1482900112640_snipaste20161228_154111.png


設定好之後，按下 Configure 按鈕，選擇編譯器版本，我選  `Visual Studio 14 2015 Win64`，再按下 Generate 產生專案檔。

如果在 build 目錄下看見 `OGREDEPS.sln` 就代表你成功了，用 VS 打開來分別把 debug/release 都編譯過一次。接著**很重要**的一點，全部編譯完之後，還要特別單獨建置一次 INSTALL 專案，debug/release 都要，這樣外部依賴庫的編譯才算完成。

## 五、下載 Ogre 源碼

跟剛剛下載外部依賴庫一樣，Ogre 源碼倉庫位於 [https://bitbucket.org/sinbad/ogre](https://bitbucket.org/sinbad/ogre)


    hg clone https://bitbucket.org/sinbad/ogre C:/OgreSDK/Ogre -r v2-1

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


