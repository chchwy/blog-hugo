---
date: 2018-10-24
title: "在 Windows 上運行 Jekyll 的兩個方法"
isCJKLanguage: true
tags: [Jekyll]
---

典型的 Jekyll 安裝方式是透過 Ruby gem 指令來進行。但是因為 Windows 系統並沒有預先安裝 Ruby 語言，所以對 Windows 使用者來說，要在本機運行 Jekyll 就成了一件麻煩事。(況且本人不是 Ruby 開發者，平常除了跑 Jekyll 外沒有機會使用 Ruby)

最近我找到了兩個辦法，可以用比較省力的方式在 Windows 上運行 Jekyll:

1. `jekyll.exe`: 打包好的單一執行檔
2. Subsystem for Linux

## 單一執行檔 `jekyll.exe`

<https://github.com/altbdoor/jekyll-exe/releases>

Github 上有人用了 Ruby 語言的打包工具將 Jekyll 打包成了可攜式的單一執行檔 `jekyll.exe`。不需要配置環境，下載下來後馬上可用，檔案只有 5.5MB，非常輕巧。

缺點是啟動速度比較慢，在我的電腦上需要10秒鐘左右來啟動。另外我發現 `--livereload` 選項時會出錯。此外整體運作的相當好，沒什麼問題。

## Subsystem for Linux

如果你的 Windows 系統上恰好有 Subsystem for Linux 的話，那也可以透過這個方式來安裝 Jekyll。用 Linux 上安裝 Jekyll 相當簡單，就是一行命令行：

```
sudo gem install bundler jekyll
```

Windows 官方建議不要把透過 Windows 應用程式去修改 Linux 系統內的檔案。但是反過來，使用 Linux 程式來操作 Windows 檔案系統的檔案是允許的。

Windows 系統的磁碟機會附掛在 Linux subsystem 的 `/mnt/` 目錄下。比如 C 槽就是 `/mnt/c/`，D 槽就是 `/mnt/d/`，依此類推。

所以，想要預覽位於 `D:\path\my-jekyll-site` 的 Jekyll 網站，可以這樣做:

```
cd /mnt/d/path/my-jekyll-site
jekyll server --livereload
```

運行後打開瀏覽器，就可以看見 Jekyll 網頁跑起來了。

這個作法的優點是運行純正的 Jekyll，效能比較好。而且理論上所有的選項和 plugin 都能運作。但是如果你平常並不需要 Linux subsystem，只為了 Jekyll 而去安裝龐大的 Linux subsystem 可能就殺雞用牛刀了。
