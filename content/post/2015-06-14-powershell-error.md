---
date: "2015-06-14"
title: "「Powershell 檔案無法載入，因為這個系統上已停用指令碼執行」"
tags: [ PowerShell ]
---

這是第一次寫 PowerShell 必然會碰到的錯誤，原因也很簡單，因為 Windows 7/8 默認情況下禁止用戶執行任何 PowerShell Script 檔案(.ps1) ，大概是怕用戶不小心執行了來路不明的檔案。

手動打開權限的作法如下：

用「系統管理員」身份打開 PowerShell (記得一定要用系統管理員) 再輸入以下命令：

```code
Set-ExecutionPolicy RemoteSigned
```

這樣就可以順利解鎖 PowerShell Script 了。

稍微解釋一下，Execution Policy 是 PowerShell 的安全管制機制。權限改成 `Remote Signed` 的意思是從網路上下載的 .ps1 要檢查數位簽章。但是本地的 PowerShell 檔案直接放行。

更多 Execution Policy 的等級可以參考[這裡][0] 。

[0]: http://gelis-dotnet.blogspot.tw/2010/10/win72008-server-powershell.html