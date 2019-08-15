---
date: 2019-08-14
isCJKLanguage: true
title: "Chocolatey 套件管理，重灌 Windows 後的好幫手"
tags: [開發日常]
---

上個月又重灌了 Windows。重灌之後，我試著挑戰一件事：不從網路上手動下載任何安裝檔，就把電腦設置回可工作狀態。

每次重灌電腦後，就是惡夢般的漫長的軟體安裝之旅。去每個軟體的官方網站下載，安裝，有時候還要重新開機。這個過程可能比安裝 Windows 本身還要漫長。

事後證明還沒辦法百分之百達成。但是除了少數例外，我已經可以安裝八成以上的常用軟體，不需要去一個一個下載安裝檔了。主要的來源有兩個：Chocolatey 和 Microsoft Store。

## Chocolatey 教學

[Chocolatey][choco] 是一個命令列的套件管理工具。類似 Ubuntu 的 `apt-get` 一個命令搞定軟體安裝，同樣的概念在 Windows 上就是 Chocolatey。許多常見的 Windows 軟體都已經在 Chocolatey 上架。

[choco]: https://chocolatey.org/ "Chocolatey Official Website"

### 安裝 Chocolatey

用有管理員權限的 Powershell，複製貼上官網的命令 :<br/>
<https://chocolatey.org/install#install-with-powershellexe>

### 常用命令

Chocolatey 的命令名稱是 `choco`，大多數操作需要管理員權限。

安裝套件 (以 7-zip 為例)
```
choco install 7zip
```

反安裝套件
```
choco uninstall 7zip
```

列出已安裝套件
```
choco list --local
```

檢查套件是否有新版本
```
choco outdated
```

更新所有套件到最新版
```
choco upgrade all
```

這是我的 Chocolatey 安裝清單：
```powershell
choco install -y 7zip                # 萬用壓縮軟體
choco install -y firefox             # 我的慣用瀏覽器
choco install -y googlechrome        # 有時候需要另一個瀏覽器
choco install -y everything          # 最強大的檔案搜尋工具
choco install -y dropbox             # Dropbox 客戶端
choco install -y k-litecodecpackfull # K-lite 萬用影音播放 codec
choco install -y nodejs-lts          # Node.js 穩定版
choco install -y python2             # Python 2.x
choco install -y golang              # Go lang
choco install -y cmake               # CMake
choco install -y vscode              # VS Code
choco install -y notepadplusplus     # 老牌文字編輯器 Notepad++
choco install -y tortoisegit         # 小烏龜 Git
choco install -y git-fork            # 另一款輕巧的 git 圖形化工具
choco install -y fluent-terminal     # 好看的 Windows 命令列
choco install -y beyondcompare       # 超級好用的 diff/merge 工具
choco install -y filezilla           # FTP 客戶端
choco install -y foxitreader         # 我慣用的 PDF viewer
choco install -y hugo                # 靜態博客產生器
choco install -y pandoc              # 強大的文件轉換工具
choco install -y paint.net           # 免費的小畫家強化版
choco install -y workflowy           # 條列式筆記工具
choco install -y typora              # Markdown 寫作軟體
choco install -y libreoffice-fresh   # 沒辦法，有時候還是要處理.docx
choco install -y mp3tag              # MP3 標籤編輯
choco install -y treesizefree        # 追蹤硬碟空間使用
choco install -y ccleaner            # 清理垃圾檔案
choco install -y teamviewer          # Teamviewer 遠端桌面
choco install -y 4k-video-downloader # 影片/音樂抓取
choco install -y xnviewmp            # 看圖軟體
choco install -y foobar2000          # 聽歌軟體
choco install -y steam               # 娛樂軟體
```

只要跑一個 Powershell 腳本，就可以把這個清單上的軟體裝好了，升級版本也只要一個命令，異常方便。

### Microsoft Store

另外有幾款軟體，雖然也可以用 Chocolatey，但是我發現 Microsoft Store 上的版本比較穩定。像是 Evernote 和 Line 的桌面客戶端。用 Microsoft Store 安裝軟體的體驗也很好，一鍵安裝，無縫升級。

### 其他沒辦法自動安裝的軟體

因為某些特定原因，以下軟體我還是必須手動安裝：

- Visual Studio (沒有其他安裝方法)
- Qt (沒有其他安裝方法)
- git (安裝過程中我需要修改選項)
- Slack (我刻意安裝 32-bit 的版本，希望能限制記憶體用量，但是還沒驗證過有沒有效XD)
