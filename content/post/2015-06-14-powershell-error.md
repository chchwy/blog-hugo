---
date: "2015-06-14"
isCJKLanguage: true
title: "「因為這個系統上已停用指令碼執行，所以無法載入」"
tags: [PowerShell]
---

每個學 PowerShell 新手必然碰到的錯誤。

因為安全性考量，Windows 預設禁止執行 PowerShell 檔案(.ps1)，同樣的命令，打在命令視窗可以，但是檔案執行就會出錯。

手動打開權限的作法如下：

用「系統管理員」身份打開 PowerShell，再輸入以下命令：

```code
Set-ExecutionPolicy RemoteSigned
```

就可以解鎖 PowerShell Script 了。

`Execution Policy` 是 PowerShell 的安全機制。`Remote Signed` 的意思是從網路上抓下來的 .ps1 要檢查數位簽章，但是本地的 PowerShell 檔案直接放行。

更多 Execution Policy 等級可以參考[這裡][0]。

[0]: http://gelis-dotnet.blogspot.tw/2010/10/win72008-server-powershell.html
