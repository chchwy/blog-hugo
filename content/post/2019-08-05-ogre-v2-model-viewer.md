---
date: 2019-08-05
isCJKLanguage: true
title: "Ogre3D v2.1 模型預覽器"
tags: [Ogre3D, Qt, C++, Open Source]
---

我寫了一個 Ogre v2.1 的 3D 模型預覽器。

![Ogre V2 Mesh Viewer Screenshot](/img/ogre-v2-mesh-viewer-screenshot.png)

[這是 Github Repo 連結][github] ，所有的程式碼都是開源的。

[下載頁面][download]：zip 解壓之後直接執行 `ogre-v2-mesh-viewer.exe` 就行。

[github]: https://github.com/chchwy/ogre-v2-mesh-viewer
[download]: https://github.com/chchwy/ogre-v2-mesh-viewer/releases

## 功能簡介

就是個簡單的 3D 模型預覽器，加上一些簡單的編輯功能：

- 讀取 Ogre3D 原生 3D 模型格式 (包括 v1, v2, xml 三種形式)
- 匯入 Wavefront obj 模型
- 匯入 glTF 2.0 模型
- 顯示場景樹狀結構
- 基本的 HLMS 材質編輯
  - 貼圖通道: diffuse, background diffuse, normal, roughness, metalness
  - 半透明
  - 網格 (wireframe)
  - 雙面材質 (Two-sided)
- 調整場景節點位置，包括位置、旋轉、縮放等等
- 顯示 Bounding box
- obj => .mesh 批次轉換工具

## 開發緣由

目前網路上大多數的既有資源都是針對 Ogre 第一版引擎。Ogre3D 第二版，包括 2.1 和 2.2 的文章及資源都像對較少，也找不到一個可靠的模型預覽器，所以我就自己寫了一個。

開發環境是 Visual Studio 2019。UI 界面是我的老朋友 Qt。Ogre3D 後端採用 OpenGL3+ 渲染 (用 DirectX 11 渲染也可以，只是有些貼圖的縮圖會沒辦法正確顯示)。
