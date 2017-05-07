---
date: 2017-05-07
title: 在 Windows 上編譯 Ogre 2.1
draft: false
isCJKLanguage: true
---
本文紀錄我在 Windows 上編譯 Ogre 2.1 的步驟。

Ogre 2.1 版跟之前的 1.x 版有比較大的差別，不管效能還是視覺方面都有顯著的提昇，架構也改變不少，而且官網上沒有預編譯好的 SDK，所以要使用 2.1 版就非得自己從源碼開始編譯不可。另外這篇是 Windows 平台，之後會另寫一篇說明 iOS 平台。

用到的環境與工具如下：

- 作業系統 Windows 10 Home
- 編譯器 Visual Studio 2015 Community
- 版本控制系統 hg 
- 建構系統 CMake 3.7。

## 下載 Source code

首先需要下載兩份 source code，分別是 Ogre3D 引擎本體以及第三方依賴庫，Ogre3D 用的是一個比較少見版本控制系統 hg，而非常見的 git，不熟悉 hg 的話可以直接用 [SourceTree][sourcetree] 這套 hg/git 通吃的版本控制軟體來抓原始碼。

Repo 倉庫路徑如下：

> https://bitbucket.org/cabalistic/ogredeps <br/>
> https://bitbucket.org/sinbad/ogre

下載後把這兩個 repo 放在同一個目錄下。例如我放在以下的位置：

> C:/OgreSDK/ogre <br/>
> C:/OgreSDK/ogredeps

[sourcetree]: https://www.sourcetreeapp.com/

## CMake

CMake 是一套跨平台專案建構工具，我用的版本是 [CMake 3.7.0][0]。

這是我第一次用 CMake，花了一些時間才搞懂用法。CMake 本身不能編譯專案，但是可以根據 cmake 腳本產生特定平台的專案檔，所以在 Windows 上 CMake 做的事就是生成 Visual Studio 2015 專案檔，接著我們就可以用 Visual Studio 來做開發。

[0]: https://cmake.org/  "CMake official site"

## 編譯依賴庫  ogredeps

首先編譯依賴庫，打開 **CMake-GUI**。

- **Where is the source code:** 填 ogredeps 的源碼位置
- **Where to build the binaries:** 按照慣例是源碼往下加一層子目錄 build 

![ogredeps](/img/cmake-ogredeps.png)

- 按 **Configure**，選擇編譯器版本 `Visual Studio 14 2015 Win64`
- 再按下 **Generate** ，產生 VS 專案檔
- 最後按 **Open Project** 就會打開產生好的 Visual Studio solution

- 打開 Visual Studio 之後，編譯 **ALL_BUILD** 專案，Debug/Release 都要編譯
- 接著編譯 **INSTALL** 專案，同樣 Debug/Release 都要編譯
- 都沒錯誤的話，依賴庫的編譯就算完成，可以接著編譯引擎本體了。

## 編譯 Ogre3D 引擎

編譯 Ogre 引擎之前，記得要手動切換到 2.1 的分支上

    hg update v2-1

- 跟編譯依賴庫一樣，打開 CMake GUI
- **Where is the source code:** 填 ogre 的源碼位置
- **Where to build the binaries:** 同樣往下加一層子目錄 build 

![](/img/cmake-ogre3d.png)

接著按下 Configure 按鈕之後呢，第一次應該會跳出一些錯誤訊息，別擔心，這是因為 CMake 找不到依賴庫的位置，我們可以藉由設定 **OGRE_DEPENDENCIES_DIR**  這個欄位來告訴 ogre 依賴庫的位置：

- **OGRE_DEPENDENCIES_DIR** 填入 C:/OgreSDK/ogredeps/build/ogredeps
- Paging、RTShaderSystem、Terrain 這三個模組目前尚未跟上 2.1 版的腳步，不要勾選
- OGRE_BUILD_SAMPLES2 是官方提供的 2.1 版範例程式集，勾起來
- OGRE_SIMD_SSE2 勾起來會產生 Linking error，原因不明，不勾。

![](/img/cmake-ogre3d-config.png)

- 再 Congifure 一次，應該就沒有錯誤訊息了
- Generate 產生 Ogre.sln
- Open Project 打開 Visual studio 專案檔
- 編譯 **ALL_BUILD** 專案，Debug/Release都要 
- 編譯 **INSTALL** 專案，同樣 Debug/Release都要
- 到此為止全部完成，挑選任一 Sample 執行即可

下圖是 Ogre3D 的 Sample_Forwrad3D 畫面：

![](/img/ogre3d-forward.jpg)


