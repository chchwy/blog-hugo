---
date: "2015-06-20"
title: "客製化 Visual C++ clean project 的行為"
tags: ["Visual Studio", C++]
isCJKLanguage: true
---

昨天花了一點時間研究一下，Visual C++ 該怎麼客製 Clean 的行為。

簡單講，我想要在按下 Clean Project 的時候，請 Visual Studio 「順便」幫我刪掉幾個目錄。說是順便但是其實不太容易，因為像 Build Project 這個動作，在專案設定頁有 Pre-build Event 和 Post-build Event 可以掛上額外的工作，但是 Clean 在專案設定裡就沒有相關的掛勾。至少 Visual Studio 並沒有 UI 可以直接操作。

## 研究 .vcxproj

我簡單研究了一下 vcxproj 的文件格式，我發現整個 C++ 專案的編譯流程，都是由 `Microsoft.Cpp.targets` 這個設定檔定義的。每個 .vcxproj 透過 `<Import>` 將 `Microsoft.Cpp.targets` 這個檔案引入，確立整個 C++ 專案的建構流程。

```xml
<Import Project="$(VCTargetsPath)\Microsoft.Cpp.targets" />
```

所以我打開 Microsoft.Cpp.target 文件，發現裡面又透過另一個檔案 `Microsoft.CppClean.targets` 來定義 Clean 的行為：

```xml
<Import Project="$(VCTargetsPath)\Microsoft.CppClean.targets" />
```

感覺我離目標越來越近了，再打開 Microsoft.CppClean.targets 文件，我馬上看見了兩個空的 Target tag:

```xml
<!-- BeforeCppClean: Redefine this target in your project in order to run tasks just before Clean. -->
<Target Name="BeforeCppClean">
</Target>

<!-- AfterCppClean: Redefine this target in your project in order to run tasks just after Clean. -->
<Target Name="AfterCppClean">
</Target>
```

OK，所以其實 Visual C++ 在 Clean 的流程中已經保留了 BeforeCppClean 和 AfterCppClean 兩個事件，分別會在 Clean 之前和之後觸發，我只要複寫這兩個事件就行了。

## 複寫 BeforeCppClean

現在我需要把「刪除目錄」這件事掛上 BeforeCppClean 這個 Target。作法是寫一個自己的 .target 文件，然後再透過 `<Import>` 標籤引入 vcxproj。實際上要做的工作就寫在 Target 標籤裡。

```xml
// 我自己定義的 MyCppClean.target
<Project ToolsVersion="12.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <Target Name="BeforeCppClean">            // 複寫 BeforeCppClean
    <Message Text="Delete XXXX Folder!" />  // 印出訊息
    <RemoveDir Directories="XXXX" />        // 刪除 XXXX 這個目錄
  </Target>
</Project>
```

最後我們把自定義的 target 引入專案，在 .vcxproj 中加入一行：

```xml
<Project>
  ...
  <Import Project="MyCppClean.target" />
</Project>
```

這樣子 Clean 專案時，就會刪除我指定的目錄了，可喜可賀。

### 參考連結

- [MSBuild 基本觀念](https://msdn.microsoft.com/zh-tw/library/dd393574.aspx)
- [MSBuild工作](https://msdn.microsoft.com/zh-tw/library/ms171466.aspx)
- [RemoveDir標籤](https://msdn.microsoft.com/zh-tw/library/xyfz6ddb.aspx)
- [Message標籤](https://msdn.microsoft.com/en-us/library/6yy0yx8d.aspx)
- [MSBuild Project File Schema](https://msdn.microsoft.com/en-us/library/5dy88c2e.aspx)









