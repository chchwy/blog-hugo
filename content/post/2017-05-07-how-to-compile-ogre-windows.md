---
date: 2017-05-07
title: 在 Windows 上編譯 Ogre 2.1
isCJKLanguage: true
tags: [Ogre]
---
本文紀錄我在 Windows 上編譯 Ogre 2.1 的方法跟步驟。

![Ogre3D](/img/ogre3d-logo.svg)

Ogre 2.1 版與 1.x 版相比不管效能還是視覺都有比較大的提昇。但是目前官網並不提供 2.x SDK 下載，所以要用 Ogre 2.1 的人就得自己從源碼編譯。

我的開發環境如下：

- 作業系統 Windows 10 Home
- 編譯器 Visual Studio 2015 Community
- 版本控制系統 hg 
- 建構系統 CMake 3.7。

## 下載 Source code

目前 Ogre3D 採用的是一個較小眾的版本控制系統 hg，而非常見的 git，如果不熟悉 hg 可以用 [SourceTree][sourcetree] 這套 hg/git 通吃的版本控制軟體來抓原始碼。

編譯 Ogre 2.1 需要下載兩份 source code:

1. Ogre3D 引擎本體 <https://bitbucket.org/cabalistic/ogredeps>
2. 第三方依賴庫 <https://bitbucket.org/sinbad/ogre>

下載後把這兩個 repo 丟進同一個目錄下。例如：

> C:/OgreSDK/ogre <br/>
> C:/OgreSDK/ogredeps

[sourcetree]: https://www.sourcetreeapp.com/

## CMake

Ogre 的專案建構工具是 CMake，是一套跨平台專案建構工具，我用的版本是 [CMake 3.7.0][0]。

因為這是我第一次用 CMake，花了一些時間才搞懂用法。CMake 本身不能編譯專案，但是可以根據 cmake 腳本產生特定平台的編譯文件，所以在 Windows 上， CMake 幹的事情就是生成 Visual Studio 2015 專案檔，接著我們就可以用 Visual Studio 來開發。

[0]: https://cmake.org/  "CMake official site"

## 編譯依賴庫  ogredeps

首先編譯依賴庫，打開 **CMake-GUI**。

- **Where is the source code:** 填 ogredeps 的源碼位置
- **Where to build the binaries:** 按照慣例，源碼下加一層 build 子目錄 

![ogredeps](/img/cmake-ogredeps.png)

- 按下 **Configure**，選擇編譯器版本 `Visual Studio 14 2015 Win64`
- 再按下 **Generate** ，產生 VS 專案檔
- 最後按 **Open Project** 就會打開產生好的 Visual Studio solution
- 打開 Visual Studio 之後，編譯 `ALL_BUILD` 專案，Debug/Release 都要編譯
- 接著要單獨編譯 `INSTALL` 專案，在 INSTALL 上按右鍵 Build，同樣 Debug/Release 都要編譯
- 這樣子依賴庫的編譯就算完成了。

## 編譯 Ogre3D 引擎

接著來編譯 Ogre 引擎本體。編譯 Ogre 引擎之前，記得要手動切換到 2.1 的分支上

    hg update v2-1

- 跟編譯依賴庫一樣，打開 CMake GUI
- **Where is the source code:** 填 ogre 的源碼位置
- **Where to build the binaries:** 同樣往下加一層子目錄 build 

![](/img/cmake-ogre3d.png)

按下 Configure 按鈕之後呢，應該會跳出一些錯誤訊息，別擔心，這是因為 CMake 還無法定位依賴庫的位置，我們可以藉由設定 **OGRE_DEPENDENCIES_DIR**  這個欄位來告訴 ogre 依賴庫的位置：

- **OGRE_DEPENDENCIES_DIR** 填入 `C:/OgreSDK/ogredeps/build/ogredeps`，或者你自己設定的位置。
- Windows 平台可以選擇底層使用 DirectX 或者 OpenGL3+，兩者都勾也行。
- Paging、RTShaderSystem、Terrain 這三個模組目前跟 2.1 不相容，記得**不要**勾。
- OGRE_BUILD_SAMPLES2 是 Ogre 2.1 版的官方範例集，勾起來。
- 可以把 SSE2 硬體指令集支援勾起來，數學運算會比較快。

![](/img/cmake-ogre3d-config.png)

- 再 Congifure 一次，應該就沒有錯誤訊息了
- 按下 Generate 產生 Ogre.sln
- Open Project 打開 Visual studio 專案檔
- 編譯 **ALL_BUILD** 專案，Debug/Release都要 
- 編譯 **INSTALL** 專案，同樣 Debug/Release都要
- 到此為止全部完成，挑選任一 Sample 執行即可

下圖是 Ogre3D 的其中一個範例 Forwrad3D 畫面：

![](/img/ogre3d-forward.jpg)


